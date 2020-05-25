{
 "cells": [
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "\u001b[32mimport \u001b[39m\u001b[36m$ivy.$                                   \n",
       "\u001b[39m\n",
       "\u001b[32mimport \u001b[39m\u001b[36m$ivy.$                              \u001b[39m"
      ]
     },
     "execution_count": 1,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "import $ivy.`org.apache.spark::spark-sql:2.4.5` \n",
    "import $ivy.`sh.almond::almond-spark:0.9.1`"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "\u001b[32mimport \u001b[39m\u001b[36morg.apache.log4j.{Level, Logger}\n",
       "\u001b[39m"
      ]
     },
     "execution_count": 2,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "import org.apache.log4j.{Level, Logger}\n",
    "Logger.getLogger(\"org\").setLevel(Level.OFF)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Loading spark-stubs\n",
      "Getting spark JARs\n",
      "Creating SparkSession\n"
     ]
    },
    {
     "name": "stderr",
     "output_type": "stream",
     "text": [
      "Using Spark's default log4j profile: org/apache/spark/log4j-defaults.properties\n"
     ]
    },
    {
     "data": {
      "text/html": [
       "<a target=\"_blank\" href=\"http://a48c4ed498ab:4040\">Spark UI</a>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/plain": [
       "\u001b[32mimport \u001b[39m\u001b[36morg.apache.spark.sql._\n",
       "\n",
       "\u001b[39m\n",
       "\u001b[36mspark\u001b[39m: \u001b[32mSparkSession\u001b[39m = org.apache.spark.sql.SparkSession@6c01f0eb"
      ]
     },
     "execution_count": 3,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "import org.apache.spark.sql._\n",
    "\n",
    "val spark = {\n",
    "  NotebookSparkSession.builder()\n",
    "    .master(\"local[*]\")\n",
    "    .getOrCreate()\n",
    "}\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "3 ways to create Spark RDDs\n",
    "- Parallelized Collections\n",
    "- External Datasets\n",
    "- Existig RDDs\n",
    "\n",
    "\n",
    "RDDs are inmutable\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "defined \u001b[32mfunction\u001b[39m \u001b[36msc\u001b[39m"
      ]
     },
     "execution_count": 4,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def sc = spark.sparkContext"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<script>\n",
       "var comm = Jupyter.notebook.kernel.comm_manager.new_comm('cancel-stage-2bb95b52-30b7-4526-b7c5-880d54810057', {});\n",
       "\n",
       "function cancelStage(stageId) {\n",
       "  console.log('Cancelling stage ' + stageId);\n",
       "  comm.send({ 'stageId': stageId });\n",
       "}\n",
       "</script>\n",
       "          "
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">count at cmd4.sc:2</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mrdd1\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[\u001b[32mString\u001b[39m] = data/sample.txt MapPartitionsRDD[1] at textFile at cmd4.sc:1\n",
       "\u001b[36mres4_1\u001b[39m: \u001b[32mLong\u001b[39m = \u001b[32m4L\u001b[39m"
      ]
     },
     "execution_count": 5,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "// Create an RDD from files\n",
    "\n",
    "val rdd1 = sc.textFile(\"data/sample.txt\")\n",
    "rdd1.count()\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "\u001b[36mrdd\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[\u001b[32mInt\u001b[39m] = ParallelCollectionRDD[2] at parallelize at cmd5.sc:1"
      ]
     },
     "execution_count": 6,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "// Create and RDD through Parallelized Collection\n",
    "\n",
    "val rdd = sc.parallelize(1 to 1000, 100)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "\u001b[36mnewRDD\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[\u001b[32mInt\u001b[39m] = MapPartitionsRDD[3] at map at cmd6.sc:1"
      ]
     },
     "execution_count": 7,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "// Create an RDD from existing RDDs\n",
    "val newRDD = rdd.map(data => (data *2))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">count at cmd7.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    100 / 100\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mres7\u001b[39m: \u001b[32mLong\u001b[39m = \u001b[32m1000L\u001b[39m"
      ]
     },
     "execution_count": 8,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "// Number of items\n",
    "newRDD.count()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">collect at cmd8.sc:2</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mfilteredrdd\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[\u001b[32mString\u001b[39m] = MapPartitionsRDD[4] at filter at cmd8.sc:1\n",
       "\u001b[36mres8_1\u001b[39m: \u001b[32mArray\u001b[39m[\u001b[32mString\u001b[39m] = \u001b[33mArray\u001b[39m(\u001b[32m\"11 1 2 3 4 5 6 7 8 9 10\"\u001b[39m)"
      ]
     },
     "execution_count": 9,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "// Filter operation - Create a new RDD\n",
    "val filteredrdd = rdd1.filter(line=>line.contains(\"11\"))\n",
    "filteredrdd.collect()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">count at cmd9.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mres9\u001b[39m: \u001b[32mLong\u001b[39m = \u001b[32m4L\u001b[39m"
      ]
     },
     "execution_count": 10,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "rdd1.count()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">first at cmd10.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    1 / 1\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mres10\u001b[39m: \u001b[32mString\u001b[39m = \u001b[32m\"1 2 3 4 5 6 7 8 9 10 \"\u001b[39m"
      ]
     },
     "execution_count": 11,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "// Read the first item\n",
    "rdd1.first()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">take at cmd11.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    1 / 1\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mres11\u001b[39m: \u001b[32mArray\u001b[39m[\u001b[32mString\u001b[39m] = \u001b[33mArray\u001b[39m(\n",
       "  \u001b[32m\"1 2 3 4 5 6 7 8 9 10 \"\u001b[39m,\n",
       "  \u001b[32m\"10 1 2 3 4 5 6 7 8 9 10 \"\u001b[39m,\n",
       "  \u001b[32m\"11 1 2 3 4 5 6 7 8 9 10\"\u001b[39m\n",
       ")"
      ]
     },
     "execution_count": 12,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "// REad the first 3 items\n",
    "rdd1.take(3)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 13,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "\u001b[36mres12\u001b[39m: \u001b[32mInt\u001b[39m = \u001b[32m2\u001b[39m"
      ]
     },
     "execution_count": 13,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "// Count the number of partitions\n",
    "rdd1.partitions.length"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 14,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "\u001b[36mres13\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[\u001b[32mString\u001b[39m] = data/sample.txt MapPartitionsRDD[1] at textFile at cmd4.sc:1"
      ]
     },
     "execution_count": 14,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "// Cache the files\n",
    "rdd1.cache()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 15,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">collect at cmd14.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mres14\u001b[39m: \u001b[32mArray\u001b[39m[\u001b[32mString\u001b[39m] = \u001b[33mArray\u001b[39m(\n",
       "  \u001b[32m\"1 2 3 4 5 6 7 8 9 10 \"\u001b[39m,\n",
       "  \u001b[32m\"10 1 2 3 4 5 6 7 8 9 10 \"\u001b[39m,\n",
       "  \u001b[32m\"11 1 2 3 4 5 6 7 8 9 10\"\u001b[39m,\n",
       "  \u001b[32m\"12 1 2 3 4 5 6 7 8 9 10 \"\u001b[39m\n",
       ")"
      ]
     },
     "execution_count": 15,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "rdd1.collect()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "// Word Count"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 16,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "\u001b[36msong\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[\u001b[32mString\u001b[39m] = data/eternalflame.txt MapPartitionsRDD[6] at textFile at cmd15.sc:1"
      ]
     },
     "execution_count": 16,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "val song=sc.textFile(\"data/eternalflame.txt\")"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 17,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "\u001b[36mwc\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[(\u001b[32mString\u001b[39m, \u001b[32mInt\u001b[39m)] = ShuffledRDD[9] at reduceByKey at cmd16.sc:1"
      ]
     },
     "execution_count": 17,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "val wc = song.flatMap(line => line.split(\" \")).map(word => (word,1)).reduceByKey(_+_)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 18,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">map at cmd16.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">take at cmd17.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    1 / 1\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mres17\u001b[39m: \u001b[32mArray\u001b[39m[(\u001b[32mString\u001b[39m, \u001b[32mInt\u001b[39m)] = \u001b[33mArray\u001b[39m(\n",
       "  (\u001b[32m\"are\"\u001b[39m, \u001b[32m1\u001b[39m),\n",
       "  (\u001b[32m\"this\"\u001b[39m, \u001b[32m7\u001b[39m),\n",
       "  (\u001b[32m\"pain\"\u001b[39m, \u001b[32m2\u001b[39m),\n",
       "  (\u001b[32m\"is\"\u001b[39m, \u001b[32m2\u001b[39m),\n",
       "  (\u001b[32m\"believe\"\u001b[39m, \u001b[32m1\u001b[39m)\n",
       ")"
      ]
     },
     "execution_count": 18,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "wc.take(5)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 19,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">collect at cmd18.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mres18\u001b[39m: \u001b[32mArray\u001b[39m[\u001b[32mString\u001b[39m] = \u001b[33mArray\u001b[39m(\n",
       "  \u001b[32m\"Close your eyes, give me your hand, darling\"\u001b[39m,\n",
       "  \u001b[32m\"Do you feel my heart beating\"\u001b[39m,\n",
       "  \u001b[32m\"Do you understand?\"\u001b[39m,\n",
       "  \u001b[32m\"Do you feel the same\"\u001b[39m,\n",
       "  \u001b[32m\"Am I only dreaming\"\u001b[39m,\n",
       "  \u001b[32m\"Is this burning an eternal flame?\"\u001b[39m,\n",
       "  \u001b[32m\"I believe it's meant to be, darling\"\u001b[39m,\n",
       "  \u001b[32m\"I watch you when you are sleeping\"\u001b[39m,\n",
       "  \u001b[32m\"You belong with me\"\u001b[39m,\n",
       "  \u001b[32m\"Do you feel the same?\"\u001b[39m,\n",
       "  \u001b[32m\"Am I only dreaming\"\u001b[39m,\n",
       "  \u001b[32m\"Or is this burning an eternal flame?\"\u001b[39m,\n",
       "  \u001b[32m\"Say my name\"\u001b[39m,\n",
       "  \u001b[32m\"Sun shines through the rain\"\u001b[39m,\n",
       "  \u001b[32m\"A whole life so lonely\"\u001b[39m,\n",
       "  \u001b[32m\"And then come and ease the pain\"\u001b[39m,\n",
       "  \u001b[32m\"I don't want to lose this feeling, oh\"\u001b[39m,\n",
       "  \u001b[32m\"Say my name\"\u001b[39m,\n",
       "  \u001b[32m\"Sun shines through the rain\"\u001b[39m,\n",
       "  \u001b[32m\"A whole life so lonely\"\u001b[39m,\n",
       "  \u001b[32m\"And then come and ease the pain\"\u001b[39m,\n",
       "  \u001b[32m\"I don't want to lose this feeling, oh\"\u001b[39m,\n",
       "  \u001b[32m\"Close your eyes, give me your hand\"\u001b[39m,\n",
       "  \u001b[32m\"Do you feel my heart beating\"\u001b[39m,\n",
       "  \u001b[32m\"Do you understand?\"\u001b[39m,\n",
       "  \u001b[32m\"Do you feel the same\"\u001b[39m,\n",
       "  \u001b[32m\"Am I only dreaming\"\u001b[39m,\n",
       "  \u001b[32m\"Or is this burning an eternal flame?\"\u001b[39m,\n",
       "  \u001b[32m\"Close your eyes, give me your hand, darling\"\u001b[39m,\n",
       "  \u001b[32m\"Do you feel my heart beating\"\u001b[39m,\n",
       "  \u001b[32m\"Do you understand?\"\u001b[39m,\n",
       "  \u001b[32m\"Do you feel the same\"\u001b[39m,\n",
       "  \u001b[32m\"Am I only dreaming\"\u001b[39m,\n",
       "  \u001b[32m\"Is this burning an eternal flame?\"\u001b[39m,\n",
       "  \u001b[32m\"Close your eyes, give me your hand, darling\"\u001b[39m,\n",
       "  \u001b[32m\"Do you feel my heart beating\"\u001b[39m,\n",
       "  \u001b[32m\"Do you understand?\"\u001b[39m,\n",
       "  \u001b[32m\"Do you feel the same\"\u001b[39m,\n",
       "..."
      ]
     },
     "execution_count": 19,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "song.collect()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Spark context: \n",
    "It allows your Spark Application to access Spark Cluster with the help of Resource Manager.\n",
    "\n",
    "The resource manager can be one of these three- Spark Standalone, YARN, Apache Mesos.\n",
    "\n",
    "Only one SparkContext may be active per JVM. You must stop the active it before creating a new one as below:\n",
    "stop()\n",
    "\n",
    "A stage is nothing but a step in a physical execution plan\n",
    "\n",
    "Executors in Spark are worker nodes"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "RDD -> Resilient Distributed Dataset \n",
    "\n",
    "Spark exposes RDD through language integrated API. In integrated API each data set is represented as an object and transformation is involved using the method of these objects.\n",
    "\n",
    "Lazy Evaluation: Spark computes transformations when an action requires a result for the driver program.\n",
    "\n",
    "\n",
    "\n",
    "Spark RDD Operations:\n",
    "\n",
    "\n",
    " 1.  Transformations :  are functions that take an RDD as the input and produce one or many RDDs as the output (Lazy)  e.g. Map(), filter(), reduceByKey() \n",
    "   \n",
    "    -- Narrow transformations : Is the result of map, filter and such that the data is from a single partition only.\n",
    "    Eg. Map, FlatMAp, MapPartition, Filter, Sample, Union\n",
    "    \n",
    "    -- Wide Transformation: Is the resulto of grouppByKey() and reduceByKey() like functions. This could come from multiple partitions.\n",
    "    \n",
    "    Eg. Intersect, Distinct, ReduceByKey, GroupByKey, Join, Cartesian, Repartition, Coalesce\n",
    "    \n",
    "    \n",
    " 2. Actions: Returns final results of RDD computations.  An Action is one of the ways to send result from executors to the driver. First(), take(), reduce(), collect(), the count() is some of the Actions in spark.\n",
    " \n",
    " \n",
    " \n",
    "\n",
    "Unlike Dataframe and datasets, RDDs don’t infer the schema of the ingested data and requires the user to specify it.\n",
    "\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 20,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "\u001b[36mres19\u001b[39m: \u001b[32mcollection\u001b[39m.\u001b[32mMap\u001b[39m[\u001b[32mInt\u001b[39m, \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[\u001b[32m_\u001b[39m]] = \u001b[33mMap\u001b[39m(\n",
       "  \u001b[32m1\u001b[39m -> data/sample.txt MapPartitionsRDD[1] at textFile at cmd4.sc:1\n",
       ")"
      ]
     },
     "execution_count": 20,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "// RDDs on cache\n",
    "sc.getPersistentRDDs"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## RDDs creation\n",
    "#### 1. Parallelized collection"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 21,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">sortByKey at cmd20.sc:2</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">parallelize at cmd20.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">foreach at cmd20.sc:3</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "(computer,65)\n",
      "(english,75)\n",
      "(maths,52)\n",
      "(maths,85)\n",
      "(science,82)\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mdata\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[(\u001b[32mString\u001b[39m, \u001b[32mInt\u001b[39m)] = ParallelCollectionRDD[10] at parallelize at cmd20.sc:1\n",
       "\u001b[36msorted\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[(\u001b[32mString\u001b[39m, \u001b[32mInt\u001b[39m)] = ShuffledRDD[13] at sortByKey at cmd20.sc:2"
      ]
     },
     "execution_count": 21,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "\n",
    "\n",
    "val data=spark.sparkContext.parallelize(Seq((\"maths\",52),(\"english\",75),(\"science\",82), (\"computer\",65),(\"maths\",85)))\n",
    "val sorted = data.sortByKey()\n",
    "sorted.foreach(println)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 22,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">collect at cmd21.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mres21\u001b[39m: \u001b[32mArray\u001b[39m[(\u001b[32mString\u001b[39m, \u001b[32mInt\u001b[39m)] = \u001b[33mArray\u001b[39m(\n",
       "  (\u001b[32m\"maths\"\u001b[39m, \u001b[32m52\u001b[39m),\n",
       "  (\u001b[32m\"english\"\u001b[39m, \u001b[32m75\u001b[39m),\n",
       "  (\u001b[32m\"science\"\u001b[39m, \u001b[32m82\u001b[39m),\n",
       "  (\u001b[32m\"computer\"\u001b[39m, \u001b[32m65\u001b[39m),\n",
       "  (\u001b[32m\"maths\"\u001b[39m, \u001b[32m85\u001b[39m)\n",
       ")"
      ]
     },
     "execution_count": 22,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data.collect"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 23,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">foreach at cmd22.sc:4</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "mar\n",
      "april\n",
      "may\n",
      "jun\n",
      "jan\n",
      "feb\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mrdd1\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[\u001b[32mString\u001b[39m] = ParallelCollectionRDD[14] at parallelize at cmd22.sc:1\n",
       "\u001b[36mresult\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[\u001b[32mString\u001b[39m] = CoalescedRDD[15] at coalesce at cmd22.sc:3"
      ]
     },
     "execution_count": 23,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "// To set the number of partitions (3)\n",
    "\n",
    "val rdd1 = spark.sparkContext.parallelize(Array(\"jan\",\"feb\",\"mar\",\"april\",\"may\",\"jun\"),3)\n",
    "// coalesce reduce the number of partitions\n",
    "val result = rdd1.coalesce(2)\n",
    "result.foreach(println)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#### 2. External Dataset"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 24,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">csv at cmd23.sc:2</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    1 / 1\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">collect at cmd23.sc:4</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    1 / 1\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/plain": [
       "\u001b[32mimport \u001b[39m\u001b[36morg.apache.spark.sql.SparkSession\n",
       "\u001b[39m\n",
       "\u001b[36mdataRDD\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[\u001b[32mRow\u001b[39m] = MapPartitionsRDD[30] at rdd at cmd23.sc:2\n",
       "\u001b[36mres23_2\u001b[39m: \u001b[32mArray\u001b[39m[\u001b[32mRow\u001b[39m] = \u001b[33mArray\u001b[39m(\n",
       "  [name, origin, cellphone],\n",
       "  [person1, bolivia, 123456],\n",
       "  [person2, spain, 484737],\n",
       "  [person3, peru, 34234324]\n",
       ")"
      ]
     },
     "execution_count": 24,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "import org.apache.spark.sql.SparkSession\n",
    "val dataRDD = spark.read.csv(\"data/sample.csv\").rdd\n",
    "// Here .rdd method is used to convert Dataset<Row> to RDD<Row>.\n",
    "dataRDD.collect()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 25,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">json at cmd24.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    1 / 1\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">collect at cmd24.sc:2</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    1 / 1\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mdataJSONRDD\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[\u001b[32mRow\u001b[39m] = MapPartitionsRDD[39] at rdd at cmd24.sc:1\n",
       "\u001b[36mres24_1\u001b[39m: \u001b[32mArray\u001b[39m[\u001b[32mRow\u001b[39m] = \u001b[33mArray\u001b[39m([Red,Apple,Large])"
      ]
     },
     "execution_count": 25,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "val dataJSONRDD = spark.read.json(\"data/sample.json\").rdd\n",
    "dataJSONRDD.collect()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 26,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">first at cmd25.sc:2</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    1 / 1\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mdataTEXTRDD\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[\u001b[32mString\u001b[39m] = MapPartitionsRDD[44] at rdd at cmd25.sc:1\n",
       "\u001b[36mres25_1\u001b[39m: \u001b[32mString\u001b[39m = \u001b[32m\"1 2 3 4 5 6 7 8 9 10 \"\u001b[39m"
      ]
     },
     "execution_count": 26,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "val dataTEXTRDD = spark.read.textFile(\"data/sample.txt\").rdd\n",
    "dataTEXTRDD.first()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#### 3. Creating RDD from existing RDD\n",
    "\n",
    "Transformation mutates one RDD into another RDD. Some of the operations applied on RDD are: filter, count, distinct, Map, FlatMap etc."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 27,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">foreach at cmd26.sc:3</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "(j,jumps)\n",
      "(o,over)\n",
      "(t,the)\n",
      "(l,lazy)\n",
      "(d,dog)\n",
      "(t,the)\n",
      "(q,quick)\n",
      "(b,brown)\n",
      "(f,fox)\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mwords\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[\u001b[32mString\u001b[39m] = ParallelCollectionRDD[45] at parallelize at cmd26.sc:1\n",
       "\u001b[36mwordPair\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[(\u001b[32mChar\u001b[39m, \u001b[32mString\u001b[39m)] = MapPartitionsRDD[46] at map at cmd26.sc:2"
      ]
     },
     "execution_count": 27,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "val words=spark.sparkContext.parallelize(Seq(\"the\", \"quick\", \"brown\", \"fox\", \"jumps\", \"over\", \"the\", \"lazy\", \"dog\"))\n",
    "val wordPair = words.map(w => (w.charAt(0), w))\n",
    "wordPair.foreach(println)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Spark Paired RDDs\n",
    "\n",
    "are nothing but RDDs containing a key-value pair. Basically, key-value pair (KVP) consists of a two linked data item in it. Here, the key is the identifier, whereas value is the data corresponding to the key value.\n",
    "\n",
    "There are several ways to create Paired RDD in Spark, like by running a map() function that returns key-value pairs."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 28,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">map at cmd27.sc:2</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">sortByKey at cmd27.sc:3</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">reduceByKey at cmd27.sc:3</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">collect at cmd27.sc:4</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mlines22\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[\u001b[32mString\u001b[39m] = data/eternalflame.txt MapPartitionsRDD[48] at textFile at cmd27.sc:1\n",
       "\u001b[36mpairs22\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[(\u001b[32mString\u001b[39m, \u001b[32mInt\u001b[39m)] = MapPartitionsRDD[49] at map at cmd27.sc:2\n",
       "\u001b[36mcounts1\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[(\u001b[32mString\u001b[39m, \u001b[32mInt\u001b[39m)] = ShuffledRDD[53] at sortByKey at cmd27.sc:3\n",
       "\u001b[36mres27_3\u001b[39m: \u001b[32mArray\u001b[39m[(\u001b[32mString\u001b[39m, \u001b[32mInt\u001b[39m)] = \u001b[33mArray\u001b[39m(\n",
       "  (\u001b[32m\"A whole life so lonely\"\u001b[39m, \u001b[32m2\u001b[39m),\n",
       "  (\u001b[32m\"Am I only dreaming\"\u001b[39m, \u001b[32m4\u001b[39m),\n",
       "  (\u001b[32m\"Am I only dreaming, ah\"\u001b[39m, \u001b[32m2\u001b[39m),\n",
       "  (\u001b[32m\"An eternal flame\"\u001b[39m, \u001b[32m1\u001b[39m),\n",
       "  (\u001b[32m\"And then come and ease the pain\"\u001b[39m, \u001b[32m2\u001b[39m),\n",
       "  (\u001b[32m\"Close your eyes, give me your hand\"\u001b[39m, \u001b[32m1\u001b[39m),\n",
       "  (\u001b[32m\"Close your eyes, give me your hand, darling\"\u001b[39m, \u001b[32m5\u001b[39m),\n",
       "  (\u001b[32m\"Do you feel my heart beating\"\u001b[39m, \u001b[32m5\u001b[39m),\n",
       "  (\u001b[32m\"Do you feel the same\"\u001b[39m, \u001b[32m5\u001b[39m),\n",
       "  (\u001b[32m\"Do you feel the same?\"\u001b[39m, \u001b[32m1\u001b[39m),\n",
       "  (\u001b[32m\"Do you understand?\"\u001b[39m, \u001b[32m5\u001b[39m),\n",
       "  (\u001b[32m\"I believe it's meant to be, darling\"\u001b[39m, \u001b[32m1\u001b[39m),\n",
       "  (\u001b[32m\"I don't want to lose this feeling, oh\"\u001b[39m, \u001b[32m2\u001b[39m),\n",
       "  (\u001b[32m\"I watch you when you are sleeping\"\u001b[39m, \u001b[32m1\u001b[39m),\n",
       "  (\u001b[32m\"Is this burning an eternal flame?\"\u001b[39m, \u001b[32m3\u001b[39m),\n",
       "  (\u001b[32m\"Or is this burning an eternal flame?\"\u001b[39m, \u001b[32m2\u001b[39m),\n",
       "  (\u001b[32m\"Say my name\"\u001b[39m, \u001b[32m2\u001b[39m),\n",
       "  (\u001b[32m\"Sun shines through the rain\"\u001b[39m, \u001b[32m2\u001b[39m),\n",
       "  (\u001b[32m\"You belong with me\"\u001b[39m, \u001b[32m1\u001b[39m)\n",
       ")"
      ]
     },
     "execution_count": 28,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "val lines22 = sc.textFile(\"data/eternalflame.txt\")\n",
    "val pairs22= lines22.map(s => (s, 1))\n",
    "val counts1 = pairs22.reduceByKey((a, b) => a + b).sortByKey()\n",
    "counts1.collect()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 29,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">take at cmd28.sc:2</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    1 / 1\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mpairs\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[(\u001b[32mString\u001b[39m, \u001b[32mString\u001b[39m)] = MapPartitionsRDD[54] at map at cmd28.sc:1\n",
       "\u001b[36mres28_1\u001b[39m: \u001b[32mArray\u001b[39m[(\u001b[32mString\u001b[39m, \u001b[32mString\u001b[39m)] = \u001b[33mArray\u001b[39m(\n",
       "  (\u001b[32m\"C\"\u001b[39m, \u001b[32m\"Close your eyes, give me your hand, darling\"\u001b[39m),\n",
       "  (\u001b[32m\"D\"\u001b[39m, \u001b[32m\"Do you feel my heart beating\"\u001b[39m),\n",
       "  (\u001b[32m\"D\"\u001b[39m, \u001b[32m\"Do you understand?\"\u001b[39m)\n",
       ")"
      ]
     },
     "execution_count": 29,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "// Let's create Spark paired RDD\n",
    "val pairs = lines22.map(x => (x.split(\"\")(0), x))\n",
    "pairs.take(3)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#### Transformation Operations"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 30,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">map at cmd28.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">collect at cmd29.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mres29\u001b[39m: \u001b[32mArray\u001b[39m[(\u001b[32mString\u001b[39m, \u001b[32mIterable\u001b[39m[\u001b[32mString\u001b[39m])] = \u001b[33mArray\u001b[39m(\n",
       "  (\n",
       "    \u001b[32m\"D\"\u001b[39m,\n",
       "    \u001b[33mCompactBuffer\u001b[39m(\n",
       "      \u001b[32m\"Do you feel my heart beating\"\u001b[39m,\n",
       "      \u001b[32m\"Do you understand?\"\u001b[39m,\n",
       "      \u001b[32m\"Do you feel the same\"\u001b[39m,\n",
       "      \u001b[32m\"Do you feel the same?\"\u001b[39m,\n",
       "      \u001b[32m\"Do you feel my heart beating\"\u001b[39m,\n",
       "      \u001b[32m\"Do you understand?\"\u001b[39m,\n",
       "      \u001b[32m\"Do you feel the same\"\u001b[39m,\n",
       "      \u001b[32m\"Do you feel my heart beating\"\u001b[39m,\n",
       "      \u001b[32m\"Do you understand?\"\u001b[39m,\n",
       "      \u001b[32m\"Do you feel the same\"\u001b[39m,\n",
       "      \u001b[32m\"Do you feel my heart beating\"\u001b[39m,\n",
       "      \u001b[32m\"Do you understand?\"\u001b[39m,\n",
       "      \u001b[32m\"Do you feel the same\"\u001b[39m,\n",
       "      \u001b[32m\"Do you feel my heart beating\"\u001b[39m,\n",
       "      \u001b[32m\"Do you understand?\"\u001b[39m,\n",
       "      \u001b[32m\"Do you feel the same\"\u001b[39m\n",
       "    )\n",
       "  ),\n",
       "  (\n",
       "    \u001b[32m\"O\"\u001b[39m,\n",
       "    \u001b[33mCompactBuffer\u001b[39m(\n",
       "      \u001b[32m\"Or is this burning an eternal flame?\"\u001b[39m,\n",
       "      \u001b[32m\"Or is this burning an eternal flame?\"\u001b[39m\n",
       "    )\n",
       "  ),\n",
       "  (\n",
       "    \u001b[32m\"A\"\u001b[39m,\n",
       "    \u001b[33mCompactBuffer\u001b[39m(\n",
       "      \u001b[32m\"Am I only dreaming\"\u001b[39m,\n",
       "      \u001b[32m\"Am I only dreaming\"\u001b[39m,\n",
       "      \u001b[32m\"A whole life so lonely\"\u001b[39m,\n",
       "      \u001b[32m\"And then come and ease the pain\"\u001b[39m,\n",
       "      \u001b[32m\"A whole life so lonely\"\u001b[39m,\n",
       "      \u001b[32m\"And then come and ease the pain\"\u001b[39m,\n",
       "      \u001b[32m\"Am I only dreaming\"\u001b[39m,\n",
       "..."
      ]
     },
     "execution_count": 30,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "pairs.groupByKey().collect()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 31,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">map at cmd28.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">collect at cmd30.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mres30\u001b[39m: \u001b[32mArray\u001b[39m[(\u001b[32mString\u001b[39m, \u001b[32mString\u001b[39m)] = \u001b[33mArray\u001b[39m(\n",
       "  (\n",
       "    \u001b[32m\"D\"\u001b[39m,\n",
       "    \u001b[32m\"Do you feel my heart beatingDo you understand?Do you feel the sameDo you feel the same?Do you feel my heart beatingDo you understand?Do you feel the sameDo you feel my heart beatingDo you understand?Do you feel the sameDo you feel my heart beatingDo you understand?Do you feel the sameDo you feel my heart beatingDo you understand?Do you feel the same\"\u001b[39m\n",
       "  ),\n",
       "  (\n",
       "    \u001b[32m\"O\"\u001b[39m,\n",
       "    \u001b[32m\"Or is this burning an eternal flame?Or is this burning an eternal flame?\"\u001b[39m\n",
       "  ),\n",
       "  (\n",
       "    \u001b[32m\"A\"\u001b[39m,\n",
       "    \u001b[32m\"Am I only dreamingAm I only dreamingA whole life so lonelyAnd then come and ease the painA whole life so lonelyAnd then come and ease the painAm I only dreamingAm I only dreamingAm I only dreaming, ahAn eternal flameAm I only dreaming, ah\"\u001b[39m\n",
       "  ),\n",
       "  (\n",
       "    \u001b[32m\"I\"\u001b[39m,\n",
       "    \u001b[32m\"Is this burning an eternal flame?I believe it's meant to be, darlingI watch you when you are sleepingI don't want to lose this feeling, ohI don't want to lose this feeling, ohIs this burning an eternal flame?Is this burning an eternal flame?\"\u001b[39m\n",
       "  ),\n",
       "  (\n",
       "    \u001b[32m\"S\"\u001b[39m,\n",
       "    \u001b[32m\"Say my nameSun shines through the rainSay my nameSun shines through the rain\"\u001b[39m\n",
       "  ),\n",
       "  (\u001b[32m\"Y\"\u001b[39m, \u001b[32m\"You belong with me\"\u001b[39m),\n",
       "  (\n",
       "    \u001b[32m\"C\"\u001b[39m,\n",
       "    \u001b[32m\"Close your eyes, give me your hand, darlingClose your eyes, give me your handClose your eyes, give me your hand, darlingClose your eyes, give me your hand, darlingClose your eyes, give me your hand, darlingClose your eyes, give me your hand, darling\"\u001b[39m\n",
       "..."
      ]
     },
     "execution_count": 31,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "pairs.reduceByKey((x, y) => x+y).collect"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 31,
   "metadata": {},
   "outputs": [],
   "source": [
    "// combineByKey"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 32,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">collect at cmd31.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mres31\u001b[39m: \u001b[32mArray\u001b[39m[(\u001b[32mString\u001b[39m, \u001b[32mString\u001b[39m)] = \u001b[33mArray\u001b[39m(\n",
       "  (\u001b[32m\"C\"\u001b[39m, \u001b[32m\"Close your eyes, give me your hand, darling1\"\u001b[39m),\n",
       "  (\u001b[32m\"D\"\u001b[39m, \u001b[32m\"Do you feel my heart beating1\"\u001b[39m),\n",
       "  (\u001b[32m\"D\"\u001b[39m, \u001b[32m\"Do you understand?1\"\u001b[39m),\n",
       "  (\u001b[32m\"D\"\u001b[39m, \u001b[32m\"Do you feel the same1\"\u001b[39m),\n",
       "  (\u001b[32m\"A\"\u001b[39m, \u001b[32m\"Am I only dreaming1\"\u001b[39m),\n",
       "  (\u001b[32m\"I\"\u001b[39m, \u001b[32m\"Is this burning an eternal flame?1\"\u001b[39m),\n",
       "  (\u001b[32m\"I\"\u001b[39m, \u001b[32m\"I believe it's meant to be, darling1\"\u001b[39m),\n",
       "  (\u001b[32m\"I\"\u001b[39m, \u001b[32m\"I watch you when you are sleeping1\"\u001b[39m),\n",
       "  (\u001b[32m\"Y\"\u001b[39m, \u001b[32m\"You belong with me1\"\u001b[39m),\n",
       "  (\u001b[32m\"D\"\u001b[39m, \u001b[32m\"Do you feel the same?1\"\u001b[39m),\n",
       "  (\u001b[32m\"A\"\u001b[39m, \u001b[32m\"Am I only dreaming1\"\u001b[39m),\n",
       "  (\u001b[32m\"O\"\u001b[39m, \u001b[32m\"Or is this burning an eternal flame?1\"\u001b[39m),\n",
       "  (\u001b[32m\"S\"\u001b[39m, \u001b[32m\"Say my name1\"\u001b[39m),\n",
       "  (\u001b[32m\"S\"\u001b[39m, \u001b[32m\"Sun shines through the rain1\"\u001b[39m),\n",
       "  (\u001b[32m\"A\"\u001b[39m, \u001b[32m\"A whole life so lonely1\"\u001b[39m),\n",
       "  (\u001b[32m\"A\"\u001b[39m, \u001b[32m\"And then come and ease the pain1\"\u001b[39m),\n",
       "  (\u001b[32m\"I\"\u001b[39m, \u001b[32m\"I don't want to lose this feeling, oh1\"\u001b[39m),\n",
       "  (\u001b[32m\"S\"\u001b[39m, \u001b[32m\"Say my name1\"\u001b[39m),\n",
       "  (\u001b[32m\"S\"\u001b[39m, \u001b[32m\"Sun shines through the rain1\"\u001b[39m),\n",
       "  (\u001b[32m\"A\"\u001b[39m, \u001b[32m\"A whole life so lonely1\"\u001b[39m),\n",
       "  (\u001b[32m\"A\"\u001b[39m, \u001b[32m\"And then come and ease the pain1\"\u001b[39m),\n",
       "  (\u001b[32m\"I\"\u001b[39m, \u001b[32m\"I don't want to lose this feeling, oh1\"\u001b[39m),\n",
       "  (\u001b[32m\"C\"\u001b[39m, \u001b[32m\"Close your eyes, give me your hand1\"\u001b[39m),\n",
       "  (\u001b[32m\"D\"\u001b[39m, \u001b[32m\"Do you feel my heart beating1\"\u001b[39m),\n",
       "  (\u001b[32m\"D\"\u001b[39m, \u001b[32m\"Do you understand?1\"\u001b[39m),\n",
       "  (\u001b[32m\"D\"\u001b[39m, \u001b[32m\"Do you feel the same1\"\u001b[39m),\n",
       "  (\u001b[32m\"A\"\u001b[39m, \u001b[32m\"Am I only dreaming1\"\u001b[39m),\n",
       "  (\u001b[32m\"O\"\u001b[39m, \u001b[32m\"Or is this burning an eternal flame?1\"\u001b[39m),\n",
       "  (\u001b[32m\"C\"\u001b[39m, \u001b[32m\"Close your eyes, give me your hand, darling1\"\u001b[39m),\n",
       "  (\u001b[32m\"D\"\u001b[39m, \u001b[32m\"Do you feel my heart beating1\"\u001b[39m),\n",
       "  (\u001b[32m\"D\"\u001b[39m, \u001b[32m\"Do you understand?1\"\u001b[39m),\n",
       "  (\u001b[32m\"D\"\u001b[39m, \u001b[32m\"Do you feel the same1\"\u001b[39m),\n",
       "  (\u001b[32m\"A\"\u001b[39m, \u001b[32m\"Am I only dreaming1\"\u001b[39m),\n",
       "  (\u001b[32m\"I\"\u001b[39m, \u001b[32m\"Is this burning an eternal flame?1\"\u001b[39m),\n",
       "  (\u001b[32m\"C\"\u001b[39m, \u001b[32m\"Close your eyes, give me your hand, darling1\"\u001b[39m),\n",
       "  (\u001b[32m\"D\"\u001b[39m, \u001b[32m\"Do you feel my heart beating1\"\u001b[39m),\n",
       "  (\u001b[32m\"D\"\u001b[39m, \u001b[32m\"Do you understand?1\"\u001b[39m),\n",
       "  (\u001b[32m\"D\"\u001b[39m, \u001b[32m\"Do you feel the same1\"\u001b[39m),\n",
       "..."
      ]
     },
     "execution_count": 32,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "pairs.mapValues(x => x+1).collect()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 33,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">collect at cmd32.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mres32\u001b[39m: \u001b[32mArray\u001b[39m[\u001b[32mString\u001b[39m] = \u001b[33mArray\u001b[39m(\n",
       "  \u001b[32m\"C\"\u001b[39m,\n",
       "  \u001b[32m\"D\"\u001b[39m,\n",
       "  \u001b[32m\"D\"\u001b[39m,\n",
       "  \u001b[32m\"D\"\u001b[39m,\n",
       "  \u001b[32m\"A\"\u001b[39m,\n",
       "  \u001b[32m\"I\"\u001b[39m,\n",
       "  \u001b[32m\"I\"\u001b[39m,\n",
       "  \u001b[32m\"I\"\u001b[39m,\n",
       "  \u001b[32m\"Y\"\u001b[39m,\n",
       "  \u001b[32m\"D\"\u001b[39m,\n",
       "  \u001b[32m\"A\"\u001b[39m,\n",
       "  \u001b[32m\"O\"\u001b[39m,\n",
       "  \u001b[32m\"S\"\u001b[39m,\n",
       "  \u001b[32m\"S\"\u001b[39m,\n",
       "  \u001b[32m\"A\"\u001b[39m,\n",
       "  \u001b[32m\"A\"\u001b[39m,\n",
       "  \u001b[32m\"I\"\u001b[39m,\n",
       "  \u001b[32m\"S\"\u001b[39m,\n",
       "  \u001b[32m\"S\"\u001b[39m,\n",
       "  \u001b[32m\"A\"\u001b[39m,\n",
       "  \u001b[32m\"A\"\u001b[39m,\n",
       "  \u001b[32m\"I\"\u001b[39m,\n",
       "  \u001b[32m\"C\"\u001b[39m,\n",
       "  \u001b[32m\"D\"\u001b[39m,\n",
       "  \u001b[32m\"D\"\u001b[39m,\n",
       "  \u001b[32m\"D\"\u001b[39m,\n",
       "  \u001b[32m\"A\"\u001b[39m,\n",
       "  \u001b[32m\"O\"\u001b[39m,\n",
       "  \u001b[32m\"C\"\u001b[39m,\n",
       "  \u001b[32m\"D\"\u001b[39m,\n",
       "  \u001b[32m\"D\"\u001b[39m,\n",
       "  \u001b[32m\"D\"\u001b[39m,\n",
       "  \u001b[32m\"A\"\u001b[39m,\n",
       "  \u001b[32m\"I\"\u001b[39m,\n",
       "  \u001b[32m\"C\"\u001b[39m,\n",
       "  \u001b[32m\"D\"\u001b[39m,\n",
       "  \u001b[32m\"D\"\u001b[39m,\n",
       "  \u001b[32m\"D\"\u001b[39m,\n",
       "..."
      ]
     },
     "execution_count": 33,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "pairs.keys.collect()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 34,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">take at cmd33.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    1 / 1\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mres33\u001b[39m: \u001b[32mArray\u001b[39m[\u001b[32mString\u001b[39m] = \u001b[33mArray\u001b[39m(\n",
       "  \u001b[32m\"Close your eyes, give me your hand, darling\"\u001b[39m,\n",
       "  \u001b[32m\"Do you feel my heart beating\"\u001b[39m,\n",
       "  \u001b[32m\"Do you understand?\"\u001b[39m,\n",
       "  \u001b[32m\"Do you feel the same\"\u001b[39m,\n",
       "  \u001b[32m\"Am I only dreaming\"\u001b[39m\n",
       ")"
      ]
     },
     "execution_count": 34,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "pairs.values.take(5)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 35,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">sortByKey at cmd34.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">map at cmd28.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">collect at cmd34.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mres34\u001b[39m: \u001b[32mArray\u001b[39m[(\u001b[32mString\u001b[39m, \u001b[32mString\u001b[39m)] = \u001b[33mArray\u001b[39m(\n",
       "  (\u001b[32m\"A\"\u001b[39m, \u001b[32m\"Am I only dreaming\"\u001b[39m),\n",
       "  (\u001b[32m\"A\"\u001b[39m, \u001b[32m\"Am I only dreaming\"\u001b[39m),\n",
       "  (\u001b[32m\"A\"\u001b[39m, \u001b[32m\"A whole life so lonely\"\u001b[39m),\n",
       "  (\u001b[32m\"A\"\u001b[39m, \u001b[32m\"And then come and ease the pain\"\u001b[39m),\n",
       "  (\u001b[32m\"A\"\u001b[39m, \u001b[32m\"A whole life so lonely\"\u001b[39m),\n",
       "  (\u001b[32m\"A\"\u001b[39m, \u001b[32m\"And then come and ease the pain\"\u001b[39m),\n",
       "  (\u001b[32m\"A\"\u001b[39m, \u001b[32m\"Am I only dreaming\"\u001b[39m),\n",
       "  (\u001b[32m\"A\"\u001b[39m, \u001b[32m\"Am I only dreaming\"\u001b[39m),\n",
       "  (\u001b[32m\"A\"\u001b[39m, \u001b[32m\"Am I only dreaming, ah\"\u001b[39m),\n",
       "  (\u001b[32m\"A\"\u001b[39m, \u001b[32m\"An eternal flame\"\u001b[39m),\n",
       "  (\u001b[32m\"A\"\u001b[39m, \u001b[32m\"Am I only dreaming, ah\"\u001b[39m),\n",
       "  (\u001b[32m\"C\"\u001b[39m, \u001b[32m\"Close your eyes, give me your hand, darling\"\u001b[39m),\n",
       "  (\u001b[32m\"C\"\u001b[39m, \u001b[32m\"Close your eyes, give me your hand\"\u001b[39m),\n",
       "  (\u001b[32m\"C\"\u001b[39m, \u001b[32m\"Close your eyes, give me your hand, darling\"\u001b[39m),\n",
       "  (\u001b[32m\"C\"\u001b[39m, \u001b[32m\"Close your eyes, give me your hand, darling\"\u001b[39m),\n",
       "  (\u001b[32m\"C\"\u001b[39m, \u001b[32m\"Close your eyes, give me your hand, darling\"\u001b[39m),\n",
       "  (\u001b[32m\"C\"\u001b[39m, \u001b[32m\"Close your eyes, give me your hand, darling\"\u001b[39m),\n",
       "  (\u001b[32m\"D\"\u001b[39m, \u001b[32m\"Do you feel my heart beating\"\u001b[39m),\n",
       "  (\u001b[32m\"D\"\u001b[39m, \u001b[32m\"Do you understand?\"\u001b[39m),\n",
       "  (\u001b[32m\"D\"\u001b[39m, \u001b[32m\"Do you feel the same\"\u001b[39m),\n",
       "  (\u001b[32m\"D\"\u001b[39m, \u001b[32m\"Do you feel the same?\"\u001b[39m),\n",
       "  (\u001b[32m\"D\"\u001b[39m, \u001b[32m\"Do you feel my heart beating\"\u001b[39m),\n",
       "  (\u001b[32m\"D\"\u001b[39m, \u001b[32m\"Do you understand?\"\u001b[39m),\n",
       "  (\u001b[32m\"D\"\u001b[39m, \u001b[32m\"Do you feel the same\"\u001b[39m),\n",
       "  (\u001b[32m\"D\"\u001b[39m, \u001b[32m\"Do you feel my heart beating\"\u001b[39m),\n",
       "  (\u001b[32m\"D\"\u001b[39m, \u001b[32m\"Do you understand?\"\u001b[39m),\n",
       "  (\u001b[32m\"D\"\u001b[39m, \u001b[32m\"Do you feel the same\"\u001b[39m),\n",
       "  (\u001b[32m\"D\"\u001b[39m, \u001b[32m\"Do you feel my heart beating\"\u001b[39m),\n",
       "  (\u001b[32m\"D\"\u001b[39m, \u001b[32m\"Do you understand?\"\u001b[39m),\n",
       "  (\u001b[32m\"D\"\u001b[39m, \u001b[32m\"Do you feel the same\"\u001b[39m),\n",
       "  (\u001b[32m\"D\"\u001b[39m, \u001b[32m\"Do you feel my heart beating\"\u001b[39m),\n",
       "  (\u001b[32m\"D\"\u001b[39m, \u001b[32m\"Do you understand?\"\u001b[39m),\n",
       "  (\u001b[32m\"D\"\u001b[39m, \u001b[32m\"Do you feel the same\"\u001b[39m),\n",
       "  (\u001b[32m\"I\"\u001b[39m, \u001b[32m\"Is this burning an eternal flame?\"\u001b[39m),\n",
       "  (\u001b[32m\"I\"\u001b[39m, \u001b[32m\"I believe it's meant to be, darling\"\u001b[39m),\n",
       "  (\u001b[32m\"I\"\u001b[39m, \u001b[32m\"I watch you when you are sleeping\"\u001b[39m),\n",
       "  (\u001b[32m\"I\"\u001b[39m, \u001b[32m\"I don't want to lose this feeling, oh\"\u001b[39m),\n",
       "  (\u001b[32m\"I\"\u001b[39m, \u001b[32m\"I don't want to lose this feeling, oh\"\u001b[39m),\n",
       "..."
      ]
     },
     "execution_count": 35,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "pairs.sortByKey().collect()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "##### Actions Operations"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 36,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">countByKey at cmd35.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">countByKey at cmd35.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mres35\u001b[39m: \u001b[32mcollection\u001b[39m.\u001b[32mMap\u001b[39m[\u001b[32mString\u001b[39m, \u001b[32mLong\u001b[39m] = \u001b[33mMap\u001b[39m(\n",
       "  \u001b[32m\"Y\"\u001b[39m -> \u001b[32m1L\u001b[39m,\n",
       "  \u001b[32m\"A\"\u001b[39m -> \u001b[32m11L\u001b[39m,\n",
       "  \u001b[32m\"I\"\u001b[39m -> \u001b[32m7L\u001b[39m,\n",
       "  \u001b[32m\"C\"\u001b[39m -> \u001b[32m6L\u001b[39m,\n",
       "  \u001b[32m\"O\"\u001b[39m -> \u001b[32m2L\u001b[39m,\n",
       "  \u001b[32m\"D\"\u001b[39m -> \u001b[32m16L\u001b[39m,\n",
       "  \u001b[32m\"S\"\u001b[39m -> \u001b[32m4L\u001b[39m\n",
       ")"
      ]
     },
     "execution_count": 36,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "pairs.countByKey()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 37,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">collectAsMap at cmd36.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mres36\u001b[39m: \u001b[32mcollection\u001b[39m.\u001b[32mMap\u001b[39m[\u001b[32mString\u001b[39m, \u001b[32mString\u001b[39m] = \u001b[33mMap\u001b[39m(\n",
       "  \u001b[32m\"S\"\u001b[39m -> \u001b[32m\"Sun shines through the rain\"\u001b[39m,\n",
       "  \u001b[32m\"D\"\u001b[39m -> \u001b[32m\"Do you feel the same\"\u001b[39m,\n",
       "  \u001b[32m\"Y\"\u001b[39m -> \u001b[32m\"You belong with me\"\u001b[39m,\n",
       "  \u001b[32m\"A\"\u001b[39m -> \u001b[32m\"Am I only dreaming, ah\"\u001b[39m,\n",
       "  \u001b[32m\"I\"\u001b[39m -> \u001b[32m\"Is this burning an eternal flame?\"\u001b[39m,\n",
       "  \u001b[32m\"C\"\u001b[39m -> \u001b[32m\"Close your eyes, give me your hand, darling\"\u001b[39m,\n",
       "  \u001b[32m\"O\"\u001b[39m -> \u001b[32m\"Or is this burning an eternal flame?\"\u001b[39m\n",
       ")"
      ]
     },
     "execution_count": 37,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "pairs.collectAsMap()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 38,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">lookup at cmd37.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mres37\u001b[39m: \u001b[32mSeq\u001b[39m[\u001b[32mString\u001b[39m] = \u001b[33mWrappedArray\u001b[39m(\n",
       "  \u001b[32m\"Do you feel my heart beating\"\u001b[39m,\n",
       "  \u001b[32m\"Do you understand?\"\u001b[39m,\n",
       "  \u001b[32m\"Do you feel the same\"\u001b[39m,\n",
       "  \u001b[32m\"Do you feel the same?\"\u001b[39m,\n",
       "  \u001b[32m\"Do you feel my heart beating\"\u001b[39m,\n",
       "  \u001b[32m\"Do you understand?\"\u001b[39m,\n",
       "  \u001b[32m\"Do you feel the same\"\u001b[39m,\n",
       "  \u001b[32m\"Do you feel my heart beating\"\u001b[39m,\n",
       "  \u001b[32m\"Do you understand?\"\u001b[39m,\n",
       "  \u001b[32m\"Do you feel the same\"\u001b[39m,\n",
       "  \u001b[32m\"Do you feel my heart beating\"\u001b[39m,\n",
       "  \u001b[32m\"Do you understand?\"\u001b[39m,\n",
       "  \u001b[32m\"Do you feel the same\"\u001b[39m,\n",
       "  \u001b[32m\"Do you feel my heart beating\"\u001b[39m,\n",
       "  \u001b[32m\"Do you understand?\"\u001b[39m,\n",
       "  \u001b[32m\"Do you feel the same\"\u001b[39m\n",
       ")"
      ]
     },
     "execution_count": 38,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "pairs.lookup(\"D\")"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 38,
   "metadata": {},
   "outputs": [],
   "source": [
    "// map(func)\n",
    "\n",
    "// The map function iterates over every line in RDD and split into new RDD. Using map() transformation we take in any function, and that function is applied to every element of RDD."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 39,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "\u001b[36mdata\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[\u001b[32mString\u001b[39m] = MapPartitionsRDD[71] at rdd at cmd38.sc:1"
      ]
     },
     "execution_count": 39,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "val data = spark.read.textFile(\"data/eternalflame.txt\").rdd"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 40,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">foreach at cmd39.sc:2</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    1 / 1\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "(Close your eyes, give me your hand, darling,43)\n",
      "(Do you feel my heart beating,28)\n",
      "(Do you understand?,18)\n",
      "(Do you feel the same,20)\n",
      "(Am I only dreaming,18)\n",
      "(Is this burning an eternal flame?,33)\n",
      "(I believe it's meant to be, darling,35)\n",
      "(I watch you when you are sleeping,33)\n",
      "(You belong with me,18)\n",
      "(Do you feel the same?,21)\n",
      "(Am I only dreaming,18)\n",
      "(Or is this burning an eternal flame?,36)\n",
      "(Say my name,11)\n",
      "(Sun shines through the rain,27)\n",
      "(A whole life so lonely,22)\n",
      "(And then come and ease the pain,31)\n",
      "(I don't want to lose this feeling, oh,37)\n",
      "(Say my name,11)\n",
      "(Sun shines through the rain,27)\n",
      "(A whole life so lonely,22)\n",
      "(And then come and ease the pain,31)\n",
      "(I don't want to lose this feeling, oh,37)\n",
      "(Close your eyes, give me your hand,34)\n",
      "(Do you feel my heart beating,28)\n",
      "(Do you understand?,18)\n",
      "(Do you feel the same,20)\n",
      "(Am I only dreaming,18)\n",
      "(Or is this burning an eternal flame?,36)\n",
      "(Close your eyes, give me your hand, darling,43)\n",
      "(Do you feel my heart beating,28)\n",
      "(Do you understand?,18)\n",
      "(Do you feel the same,20)\n",
      "(Am I only dreaming,18)\n",
      "(Is this burning an eternal flame?,33)\n",
      "(Close your eyes, give me your hand, darling,43)\n",
      "(Do you feel my heart beating,28)\n",
      "(Do you understand?,18)\n",
      "(Do you feel the same,20)\n",
      "(Am I only dreaming, ah,22)\n",
      "(An eternal flame,16)\n",
      "(Close your eyes, give me your hand, darling,43)\n",
      "(Do you feel my heart beating,28)\n",
      "(Do you understand?,18)\n",
      "(Do you feel the same,20)\n",
      "(Am I only dreaming, ah,22)\n",
      "(Is this burning an eternal flame?,33)\n",
      "(Close your eyes, give me your hand, darling,43)\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mmapFile\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[(\u001b[32mString\u001b[39m, \u001b[32mInt\u001b[39m)] = MapPartitionsRDD[72] at map at cmd39.sc:1"
      ]
     },
     "execution_count": 40,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "val mapFile = data.map(line => (line, line.length))\n",
    "mapFile.foreach(println)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 41,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">foreach at cmd40.sc:2</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    1 / 1\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Close\n",
      "your\n",
      "eyes,\n",
      "give\n",
      "me\n",
      "your\n",
      "hand,\n",
      "darling\n",
      "Do\n",
      "you\n",
      "feel\n",
      "my\n",
      "heart\n",
      "beating\n",
      "Do\n",
      "you\n",
      "understand?\n",
      "Do\n",
      "you\n",
      "feel\n",
      "the\n",
      "same\n",
      "Am\n",
      "I\n",
      "only\n",
      "dreaming\n",
      "Is\n",
      "this\n",
      "burning\n",
      "an\n",
      "eternal\n",
      "flame?\n",
      "I\n",
      "believe\n",
      "it's\n",
      "meant\n",
      "to\n",
      "be,\n",
      "darling\n",
      "I\n",
      "watch\n",
      "you\n",
      "when\n",
      "you\n",
      "are\n",
      "sleeping\n",
      "You\n",
      "belong\n",
      "with\n",
      "me\n",
      "Do\n",
      "you\n",
      "feel\n",
      "the\n",
      "same?\n",
      "Am\n",
      "I\n",
      "only\n",
      "dreaming\n",
      "Or\n",
      "is\n",
      "this\n",
      "burning\n",
      "an\n",
      "eternal\n",
      "flame?\n",
      "Say\n",
      "my\n",
      "name\n",
      "Sun\n",
      "shines\n",
      "through\n",
      "the\n",
      "rain\n",
      "A\n",
      "whole\n",
      "life\n",
      "so\n",
      "lonely\n",
      "And\n",
      "then\n",
      "come\n",
      "and\n",
      "ease\n",
      "the\n",
      "pain\n",
      "I\n",
      "don't\n",
      "want\n",
      "to\n",
      "lose\n",
      "this\n",
      "feeling,\n",
      "oh\n",
      "Say\n",
      "my\n",
      "name\n",
      "Sun\n",
      "shines\n",
      "through\n",
      "the\n",
      "rain\n",
      "A\n",
      "whole\n",
      "life\n",
      "so\n",
      "lonely\n",
      "And\n",
      "then\n",
      "come\n",
      "and\n",
      "ease\n",
      "the\n",
      "pain\n",
      "I\n",
      "don't\n",
      "want\n",
      "to\n",
      "lose\n",
      "this\n",
      "feeling,\n",
      "oh\n",
      "Close\n",
      "your\n",
      "eyes,\n",
      "give\n",
      "me\n",
      "your\n",
      "hand\n",
      "Do\n",
      "you\n",
      "feel\n",
      "my\n",
      "heart\n",
      "beating\n",
      "Do\n",
      "you\n",
      "understand?\n",
      "Do\n",
      "you\n",
      "feel\n",
      "the\n",
      "same\n",
      "Am\n",
      "I\n",
      "only\n",
      "dreaming\n",
      "Or\n",
      "is\n",
      "this\n",
      "burning\n",
      "an\n",
      "eternal\n",
      "flame?\n",
      "Close\n",
      "your\n",
      "eyes,\n",
      "give\n",
      "me\n",
      "your\n",
      "hand,\n",
      "darling\n",
      "Do\n",
      "you\n",
      "feel\n",
      "my\n",
      "heart\n",
      "beating\n",
      "Do\n",
      "you\n",
      "understand?\n",
      "Do\n",
      "you\n",
      "feel\n",
      "the\n",
      "same\n",
      "Am\n",
      "I\n",
      "only\n",
      "dreaming\n",
      "Is\n",
      "this\n",
      "burning\n",
      "an\n",
      "eternal\n",
      "flame?\n",
      "Close\n",
      "your\n",
      "eyes,\n",
      "give\n",
      "me\n",
      "your\n",
      "hand,\n",
      "darling\n",
      "Do\n",
      "you\n",
      "feel\n",
      "my\n",
      "heart\n",
      "beating\n",
      "Do\n",
      "you\n",
      "understand?\n",
      "Do\n",
      "you\n",
      "feel\n",
      "the\n",
      "same\n",
      "Am\n",
      "I\n",
      "only\n",
      "dreaming,\n",
      "ah\n",
      "An\n",
      "eternal\n",
      "flame\n",
      "Close\n",
      "your\n",
      "eyes,\n",
      "give\n",
      "me\n",
      "your\n",
      "hand,\n",
      "darling\n",
      "Do\n",
      "you\n",
      "feel\n",
      "my\n",
      "heart\n",
      "beating\n",
      "Do\n",
      "you\n",
      "understand?\n",
      "Do\n",
      "you\n",
      "feel\n",
      "the\n",
      "same\n",
      "Am\n",
      "I\n",
      "only\n",
      "dreaming,\n",
      "ah\n",
      "Is\n",
      "this\n",
      "burning\n",
      "an\n",
      "eternal\n",
      "flame?\n",
      "Close\n",
      "your\n",
      "eyes,\n",
      "give\n",
      "me\n",
      "your\n",
      "hand,\n",
      "darling\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mflatmapFile\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[\u001b[32mString\u001b[39m] = MapPartitionsRDD[73] at flatMap at cmd40.sc:1"
      ]
     },
     "execution_count": 41,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "// flatMap()\n",
    "// The key difference between map() and flatMap() is map() returns only one element, while flatMap() can return a list of elements.\n",
    "\n",
    "val flatmapFile = data.flatMap(lines => lines.split(\" \"))\n",
    "flatmapFile.foreach(println)\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 42,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">collect at cmd41.sc:2</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    1 / 1\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mmapFile\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[\u001b[32mString\u001b[39m] = MapPartitionsRDD[75] at filter at cmd41.sc:1\n",
       "\u001b[36mres41_1\u001b[39m: \u001b[32mArray\u001b[39m[\u001b[32mString\u001b[39m] = \u001b[33mArray\u001b[39m(\u001b[32m\"heart\"\u001b[39m, \u001b[32m\"heart\"\u001b[39m, \u001b[32m\"heart\"\u001b[39m, \u001b[32m\"heart\"\u001b[39m, \u001b[32m\"heart\"\u001b[39m)"
      ]
     },
     "execution_count": 42,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "// filter(func)\n",
    "\n",
    "// Spark RDD filter() function returns a new RDD, containing only the elements that meet a predicate.\n",
    "\n",
    "val mapFile = data.flatMap(lines=> lines.split(\" \")).filter(value => value==\"heart\")\n",
    "mapFile.collect()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 42,
   "metadata": {},
   "outputs": [],
   "source": [
    "// mapPartitions(func)\n",
    "// The MapPartition converts each partition of the source RDD into many elements of the result (possibly none). In mapPartition(), the map() function is applied on each partitions simultaneously. MapPartition is like a map, but the difference is it runs separately on each partition(block) of the RDD"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 42,
   "metadata": {},
   "outputs": [],
   "source": [
    "// mapPartitionWithIndex()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 43,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">foreach at cmd42.sc:5</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    6 / 6\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "(3,nov,2014)\n",
      "(16,feb,2014)\n",
      "(1,jan,2016)\n",
      "(5,dec,2014)\n",
      "(17,sep,2015)\n",
      "(6,dec,2011)\n",
      "(16,may,2015)\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mrdd1\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[(\u001b[32mInt\u001b[39m, \u001b[32mString\u001b[39m, \u001b[32mInt\u001b[39m)] = ParallelCollectionRDD[76] at parallelize at cmd42.sc:1\n",
       "\u001b[36mrdd2\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[(\u001b[32mInt\u001b[39m, \u001b[32mString\u001b[39m, \u001b[32mInt\u001b[39m)] = ParallelCollectionRDD[77] at parallelize at cmd42.sc:2\n",
       "\u001b[36mrdd3\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[(\u001b[32mInt\u001b[39m, \u001b[32mString\u001b[39m, \u001b[32mInt\u001b[39m)] = ParallelCollectionRDD[78] at parallelize at cmd42.sc:3\n",
       "\u001b[36mrddUnion\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[(\u001b[32mInt\u001b[39m, \u001b[32mString\u001b[39m, \u001b[32mInt\u001b[39m)] = UnionRDD[80] at union at cmd42.sc:4"
      ]
     },
     "execution_count": 43,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "// Union\n",
    "\n",
    "val rdd1 = spark.sparkContext.parallelize(Seq((1,\"jan\",2016),(3,\"nov\",2014),(16,\"feb\",2014)))\n",
    "val rdd2 = spark.sparkContext.parallelize(Seq((5,\"dec\",2014),(17,\"sep\",2015)))\n",
    "val rdd3 = spark.sparkContext.parallelize(Seq((6,\"dec\",2011),(16,\"may\",2015)))\n",
    "val rddUnion = rdd1.union(rdd2).union(rdd3)\n",
    "rddUnion.foreach(println)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 44,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">intersection at cmd43.sc:3</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">intersection at cmd43.sc:3</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">foreach at cmd43.sc:4</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "(1,jan,2016)\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mrdd1\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[(\u001b[32mInt\u001b[39m, \u001b[32mString\u001b[39m, \u001b[32mInt\u001b[39m)] = ParallelCollectionRDD[81] at parallelize at cmd43.sc:1\n",
       "\u001b[36mrdd2\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[(\u001b[32mInt\u001b[39m, \u001b[32mString\u001b[39m, \u001b[32mInt\u001b[39m)] = ParallelCollectionRDD[82] at parallelize at cmd43.sc:2\n",
       "\u001b[36mcomman\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[(\u001b[32mInt\u001b[39m, \u001b[32mString\u001b[39m, \u001b[32mInt\u001b[39m)] = MapPartitionsRDD[88] at intersection at cmd43.sc:3"
      ]
     },
     "execution_count": 44,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "// Intersection\n",
    "\n",
    "val rdd1 = spark.sparkContext.parallelize(Seq((1,\"jan\",2016),(3,\"nov\",2014), (16,\"feb\",2014)  ))\n",
    "val rdd2 = spark.sparkContext.parallelize(Seq((5,\"dec\",2014),(1,\"jan\",2016)))\n",
    "val comman = rdd1.intersection(rdd2)\n",
    "comman.foreach(println)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 45,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">distinct at cmd44.sc:2</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">collect at cmd44.sc:3</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "(3,nov,2014), (16,feb,2014), (1,jan,2016)\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mrdd1\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[(\u001b[32mInt\u001b[39m, \u001b[32mString\u001b[39m, \u001b[32mInt\u001b[39m)] = ParallelCollectionRDD[89] at parallelize at cmd44.sc:1\n",
       "\u001b[36mresult\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[(\u001b[32mInt\u001b[39m, \u001b[32mString\u001b[39m, \u001b[32mInt\u001b[39m)] = MapPartitionsRDD[92] at distinct at cmd44.sc:2"
      ]
     },
     "execution_count": 45,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "// distinct()\n",
    "\n",
    "val rdd1 = spark.sparkContext.parallelize(Seq((1,\"jan\",2016),(3,\"nov\",2014),(16,\"feb\",2014),(3,\"nov\",2014)))\n",
    "val result = rdd1.distinct()\n",
    "println(result.collect().mkString(\", \"))\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 46,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">parallelize at cmd45.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    3 / 3\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">collect at cmd45.sc:2</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    3 / 3\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "(s,CompactBuffer(3, 4))\n",
      "(p,CompactBuffer(7, 5))\n",
      "(t,CompactBuffer(8))\n",
      "(k,CompactBuffer(5, 6))\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mdata\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[(\u001b[32mChar\u001b[39m, \u001b[32mInt\u001b[39m)] = ParallelCollectionRDD[93] at parallelize at cmd45.sc:1\n",
       "\u001b[36mgroup\u001b[39m: \u001b[32mArray\u001b[39m[(\u001b[32mChar\u001b[39m, \u001b[32mIterable\u001b[39m[\u001b[32mInt\u001b[39m])] = \u001b[33mArray\u001b[39m(\n",
       "  (\u001b[32m's'\u001b[39m, \u001b[33mCompactBuffer\u001b[39m(\u001b[32m3\u001b[39m, \u001b[32m4\u001b[39m)),\n",
       "  (\u001b[32m'p'\u001b[39m, \u001b[33mCompactBuffer\u001b[39m(\u001b[32m7\u001b[39m, \u001b[32m5\u001b[39m)),\n",
       "  (\u001b[32m't'\u001b[39m, \u001b[33mCompactBuffer\u001b[39m(\u001b[32m8\u001b[39m)),\n",
       "  (\u001b[32m'k'\u001b[39m, \u001b[33mCompactBuffer\u001b[39m(\u001b[32m5\u001b[39m, \u001b[32m6\u001b[39m))\n",
       ")"
      ]
     },
     "execution_count": 46,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "// groupByKey()\n",
    "\n",
    "val data = spark.sparkContext.parallelize(Array(('k',5),('s',3),('s',4),('p',7),('p',5),('t',8),('k',6)),3)\n",
    "val group = data.groupByKey().collect()\n",
    "group.foreach(println)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 47,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">map at cmd46.sc:2</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">foreach at cmd46.sc:3</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "(eight,1)\n",
      "(ten,1)\n",
      "(two,2)\n",
      "(one,1)\n",
      "(nine,1)\n",
      "(six,2)\n",
      "(five,1)\n",
      "(four,1)\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mwords\u001b[39m: \u001b[32mArray\u001b[39m[\u001b[32mString\u001b[39m] = \u001b[33mArray\u001b[39m(\n",
       "  \u001b[32m\"one\"\u001b[39m,\n",
       "  \u001b[32m\"two\"\u001b[39m,\n",
       "  \u001b[32m\"two\"\u001b[39m,\n",
       "  \u001b[32m\"four\"\u001b[39m,\n",
       "  \u001b[32m\"five\"\u001b[39m,\n",
       "  \u001b[32m\"six\"\u001b[39m,\n",
       "  \u001b[32m\"six\"\u001b[39m,\n",
       "  \u001b[32m\"eight\"\u001b[39m,\n",
       "  \u001b[32m\"nine\"\u001b[39m,\n",
       "  \u001b[32m\"ten\"\u001b[39m\n",
       ")\n",
       "\u001b[36mdata\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[(\u001b[32mString\u001b[39m, \u001b[32mInt\u001b[39m)] = ShuffledRDD[97] at reduceByKey at cmd46.sc:2"
      ]
     },
     "execution_count": 47,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "// reduceByKey()\n",
    "val words = Array(\"one\",\"two\",\"two\",\"four\",\"five\",\"six\",\"six\",\"eight\",\"nine\",\"ten\")\n",
    "val data = spark.sparkContext.parallelize(words).map(w => (w,1)).reduceByKey(_+_)\n",
    "data.foreach(println)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 48,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">sortByKey at cmd47.sc:2</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">parallelize at cmd47.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">foreach at cmd47.sc:3</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "(computer,65)\n",
      "(english,75)\n",
      "(maths,52)\n",
      "(maths,85)\n",
      "(science,82)\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mdata\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[(\u001b[32mString\u001b[39m, \u001b[32mInt\u001b[39m)] = ParallelCollectionRDD[98] at parallelize at cmd47.sc:1\n",
       "\u001b[36msorted\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[(\u001b[32mString\u001b[39m, \u001b[32mInt\u001b[39m)] = ShuffledRDD[101] at sortByKey at cmd47.sc:2"
      ]
     },
     "execution_count": 48,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "// sortByKey\n",
    "val data = spark.sparkContext.parallelize(Seq((\"maths\",52), (\"english\",75), (\"science\",82), (\"computer\",65), (\"maths\",85)))\n",
    "val sorted = data.sortByKey()\n",
    "sorted.foreach(println)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 49,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">parallelize at cmd48.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">parallelize at cmd48.sc:2</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">collect at cmd48.sc:4</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "(b,(2,7)),(A,(1,4)),(A,(1,6)),(c,(3,3)),(c,(3,8))\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mdata\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[(\u001b[32mChar\u001b[39m, \u001b[32mInt\u001b[39m)] = ParallelCollectionRDD[102] at parallelize at cmd48.sc:1\n",
       "\u001b[36mdata2\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[(\u001b[32mChar\u001b[39m, \u001b[32mInt\u001b[39m)] = ParallelCollectionRDD[103] at parallelize at cmd48.sc:2\n",
       "\u001b[36mresult\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[(\u001b[32mChar\u001b[39m, (\u001b[32mInt\u001b[39m, \u001b[32mInt\u001b[39m))] = MapPartitionsRDD[106] at join at cmd48.sc:3"
      ]
     },
     "execution_count": 49,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "// join()\n",
    "\n",
    "\n",
    "// The Join is database term. It combines the fields from two table using common values. join() operation in Spark is defined on pair-wise RDD. Pair-wise RDDs are RDD in which each element is in the form of tuples. Where the first element is key and the second element is the value.\n",
    "\n",
    "val data = spark.sparkContext.parallelize(Array(('A',1),('b',2),('c',3)))\n",
    "val data2 =spark.sparkContext.parallelize(Array(('A',4),('A',6),('b',7),('c',3),('c',8)))\n",
    "val result = data.join(data2)\n",
    "println(result.collect().mkString(\",\"))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 50,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">foreach at cmd49.sc:3</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "mar\n",
      "april\n",
      "may\n",
      "jun\n",
      "jan\n",
      "feb\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mrdd1\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[\u001b[32mString\u001b[39m] = ParallelCollectionRDD[107] at parallelize at cmd49.sc:1\n",
       "\u001b[36mresult\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[\u001b[32mString\u001b[39m] = CoalescedRDD[108] at coalesce at cmd49.sc:2"
      ]
     },
     "execution_count": 50,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "// coalesce() \n",
    "// To avoid full shuffling of data we use coalesce() function. In coalesce() we use existing partition so that less data is shuffled.\n",
    "\n",
    "val rdd1 = spark.sparkContext.parallelize(Array(\"jan\",\"feb\",\"mar\",\"april\",\"may\",\"jun\"),3)\n",
    "val result = rdd1.coalesce(2)\n",
    "result.foreach(println)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": 51,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">count at cmd50.sc:3</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    1 / 1\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "5\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mdata\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[\u001b[32mString\u001b[39m] = MapPartitionsRDD[113] at rdd at cmd50.sc:1\n",
       "\u001b[36mmapFile\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[\u001b[32mString\u001b[39m] = MapPartitionsRDD[115] at filter at cmd50.sc:2"
      ]
     },
     "execution_count": 51,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "// .count()\n",
    "val data = spark.read.textFile(\"data/eternalflame.txt\").rdd\n",
    "val mapFile = data.flatMap(lines => lines.split(\" \")).filter(value => value==\"heart\")\n",
    "println(mapFile.count())"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 52,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">parallelize at cmd51.sc:2</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">parallelize at cmd51.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">collect at cmd51.sc:4</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "(b,(2,7)),(A,(1,4)),(A,(1,6)),(c,(3,3)),(c,(3,8))\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mdata\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[(\u001b[32mChar\u001b[39m, \u001b[32mInt\u001b[39m)] = ParallelCollectionRDD[116] at parallelize at cmd51.sc:1\n",
       "\u001b[36mdata2\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[(\u001b[32mChar\u001b[39m, \u001b[32mInt\u001b[39m)] = ParallelCollectionRDD[117] at parallelize at cmd51.sc:2\n",
       "\u001b[36mresult\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[(\u001b[32mChar\u001b[39m, (\u001b[32mInt\u001b[39m, \u001b[32mInt\u001b[39m))] = MapPartitionsRDD[120] at join at cmd51.sc:3"
      ]
     },
     "execution_count": 52,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "// collect()\n",
    "// The action collect() is the common and simplest operation that returns our entire RDDs content to driver program\n",
    "val data = spark.sparkContext.parallelize(Array(('A',1),('b',2),('c',3)))\n",
    "val data2 =spark.sparkContext.parallelize(Array(('A',4),('A',6),('b',7),('c',3),('c',8)))\n",
    "val result = data.join(data2)\n",
    "println(result.collect().mkString(\",\"))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 53,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">parallelize at cmd52.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    3 / 3\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">collect at cmd52.sc:2</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    3 / 3\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">take at cmd52.sc:3</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    1 / 1\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">take at cmd52.sc:3</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    1 / 1\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "(b,(2,7))\n",
      "(A,(1,4))\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mdata\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[(\u001b[32mChar\u001b[39m, \u001b[32mInt\u001b[39m)] = ParallelCollectionRDD[121] at parallelize at cmd52.sc:1\n",
       "\u001b[36mgroup\u001b[39m: \u001b[32mArray\u001b[39m[(\u001b[32mChar\u001b[39m, \u001b[32mIterable\u001b[39m[\u001b[32mInt\u001b[39m])] = \u001b[33mArray\u001b[39m(\n",
       "  (\u001b[32m's'\u001b[39m, \u001b[33mCompactBuffer\u001b[39m(\u001b[32m3\u001b[39m, \u001b[32m4\u001b[39m)),\n",
       "  (\u001b[32m'p'\u001b[39m, \u001b[33mCompactBuffer\u001b[39m(\u001b[32m7\u001b[39m, \u001b[32m5\u001b[39m)),\n",
       "  (\u001b[32m't'\u001b[39m, \u001b[33mCompactBuffer\u001b[39m(\u001b[32m8\u001b[39m)),\n",
       "  (\u001b[32m'k'\u001b[39m, \u001b[33mCompactBuffer\u001b[39m(\u001b[32m5\u001b[39m, \u001b[32m6\u001b[39m))\n",
       ")\n",
       "\u001b[36mtwoRec\u001b[39m: \u001b[32mArray\u001b[39m[(\u001b[32mChar\u001b[39m, (\u001b[32mInt\u001b[39m, \u001b[32mInt\u001b[39m))] = \u001b[33mArray\u001b[39m((\u001b[32m'b'\u001b[39m, (\u001b[32m2\u001b[39m, \u001b[32m7\u001b[39m)), (\u001b[32m'A'\u001b[39m, (\u001b[32m1\u001b[39m, \u001b[32m4\u001b[39m)))"
      ]
     },
     "execution_count": 53,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "// take()\n",
    "val data = spark.sparkContext.parallelize(Array(('k',5),('s',3),('s',4),('p',7),('p',5),('t',8),('k',6)),3)\n",
    "val group = data.groupByKey().collect()\n",
    "val twoRec = result.take(2)\n",
    "twoRec.foreach(println)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 54,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">top at cmd53.sc:3</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    1 / 1\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "(You belong with me,18)\n",
      "(Sun shines through the rain,27)\n",
      "(Sun shines through the rain,27)\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mdata\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[\u001b[32mString\u001b[39m] = MapPartitionsRDD[127] at rdd at cmd53.sc:1\n",
       "\u001b[36mmapFile\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[(\u001b[32mString\u001b[39m, \u001b[32mInt\u001b[39m)] = MapPartitionsRDD[128] at map at cmd53.sc:2\n",
       "\u001b[36mres\u001b[39m: \u001b[32mArray\u001b[39m[(\u001b[32mString\u001b[39m, \u001b[32mInt\u001b[39m)] = \u001b[33mArray\u001b[39m(\n",
       "  (\u001b[32m\"You belong with me\"\u001b[39m, \u001b[32m18\u001b[39m),\n",
       "  (\u001b[32m\"Sun shines through the rain\"\u001b[39m, \u001b[32m27\u001b[39m),\n",
       "  (\u001b[32m\"Sun shines through the rain\"\u001b[39m, \u001b[32m27\u001b[39m)\n",
       ")"
      ]
     },
     "execution_count": 54,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "// top()\n",
    "// not working\n",
    "val data = spark.read.textFile(\"data/eternalFlame.txt\").rdd\n",
    "val mapFile = data.map(line => (line,line.length))\n",
    "val res = mapFile.top(3)\n",
    "res.foreach(println)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 55,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">top at cmd54.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    1 / 1\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mres54\u001b[39m: \u001b[32mArray\u001b[39m[\u001b[32mString\u001b[39m] = \u001b[33mArray\u001b[39m(\n",
       "  \u001b[32m\"You belong with me\"\u001b[39m,\n",
       "  \u001b[32m\"Sun shines through the rain\"\u001b[39m,\n",
       "  \u001b[32m\"Sun shines through the rain\"\u001b[39m\n",
       ")"
      ]
     },
     "execution_count": 55,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data.top(3)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 56,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">countByValue at cmd55.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    1 / 1\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">countByValue at cmd55.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    1 / 1\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "((I watch you when you are sleeping,33),1)\n",
      "((Do you feel the same,20),5)\n",
      "((Do you feel the same?,21),1)\n",
      "((Is this burning an eternal flame?,33),3)\n",
      "((I don't want to lose this feeling, oh,37),2)\n",
      "((Sun shines through the rain,27),2)\n",
      "((Am I only dreaming,18),4)\n",
      "((An eternal flame,16),1)\n",
      "((Close your eyes, give me your hand,34),1)\n",
      "((Do you understand?,18),5)\n",
      "((Close your eyes, give me your hand, darling,43),5)\n",
      "((Am I only dreaming, ah,22),2)\n",
      "((You belong with me,18),1)\n",
      "((I believe it's meant to be, darling,35),1)\n",
      "((And then come and ease the pain,31),2)\n",
      "((Or is this burning an eternal flame?,36),2)\n",
      "((Say my name,11),2)\n",
      "((Do you feel my heart beating,28),5)\n",
      "((A whole life so lonely,22),2)\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mresult\u001b[39m: \u001b[32mcollection\u001b[39m.\u001b[32mMap\u001b[39m[(\u001b[32mString\u001b[39m, \u001b[32mInt\u001b[39m), \u001b[32mLong\u001b[39m] = \u001b[33mMap\u001b[39m(\n",
       "  (\u001b[32m\"I watch you when you are sleeping\"\u001b[39m, \u001b[32m33\u001b[39m) -> \u001b[32m1L\u001b[39m,\n",
       "  (\u001b[32m\"Do you feel the same\"\u001b[39m, \u001b[32m20\u001b[39m) -> \u001b[32m5L\u001b[39m,\n",
       "  (\u001b[32m\"Do you feel the same?\"\u001b[39m, \u001b[32m21\u001b[39m) -> \u001b[32m1L\u001b[39m,\n",
       "  (\u001b[32m\"Is this burning an eternal flame?\"\u001b[39m, \u001b[32m33\u001b[39m) -> \u001b[32m3L\u001b[39m,\n",
       "  (\u001b[32m\"I don't want to lose this feeling, oh\"\u001b[39m, \u001b[32m37\u001b[39m) -> \u001b[32m2L\u001b[39m,\n",
       "  (\u001b[32m\"Sun shines through the rain\"\u001b[39m, \u001b[32m27\u001b[39m) -> \u001b[32m2L\u001b[39m,\n",
       "  (\u001b[32m\"Am I only dreaming\"\u001b[39m, \u001b[32m18\u001b[39m) -> \u001b[32m4L\u001b[39m,\n",
       "  (\u001b[32m\"An eternal flame\"\u001b[39m, \u001b[32m16\u001b[39m) -> \u001b[32m1L\u001b[39m,\n",
       "  (\u001b[32m\"Close your eyes, give me your hand\"\u001b[39m, \u001b[32m34\u001b[39m) -> \u001b[32m1L\u001b[39m,\n",
       "  (\u001b[32m\"Do you understand?\"\u001b[39m, \u001b[32m18\u001b[39m) -> \u001b[32m5L\u001b[39m,\n",
       "  (\u001b[32m\"Close your eyes, give me your hand, darling\"\u001b[39m, \u001b[32m43\u001b[39m) -> \u001b[32m5L\u001b[39m,\n",
       "  (\u001b[32m\"Am I only dreaming, ah\"\u001b[39m, \u001b[32m22\u001b[39m) -> \u001b[32m2L\u001b[39m,\n",
       "  (\u001b[32m\"You belong with me\"\u001b[39m, \u001b[32m18\u001b[39m) -> \u001b[32m1L\u001b[39m,\n",
       "  (\u001b[32m\"I believe it's meant to be, darling\"\u001b[39m, \u001b[32m35\u001b[39m) -> \u001b[32m1L\u001b[39m,\n",
       "  (\u001b[32m\"And then come and ease the pain\"\u001b[39m, \u001b[32m31\u001b[39m) -> \u001b[32m2L\u001b[39m,\n",
       "  (\u001b[32m\"Or is this burning an eternal flame?\"\u001b[39m, \u001b[32m36\u001b[39m) -> \u001b[32m2L\u001b[39m,\n",
       "  (\u001b[32m\"Say my name\"\u001b[39m, \u001b[32m11\u001b[39m) -> \u001b[32m2L\u001b[39m,\n",
       "  (\u001b[32m\"Do you feel my heart beating\"\u001b[39m, \u001b[32m28\u001b[39m) -> \u001b[32m5L\u001b[39m,\n",
       "  (\u001b[32m\"A whole life so lonely\"\u001b[39m, \u001b[32m22\u001b[39m) -> \u001b[32m2L\u001b[39m\n",
       ")"
      ]
     },
     "execution_count": 56,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "//countByValue\n",
    "\n",
    "val result= data.map(line => (line,line.length)).countByValue()\n",
    "result.foreach(println)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 57,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">reduce at cmd56.sc:2</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "172\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mrdd1\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[\u001b[32mInt\u001b[39m] = ParallelCollectionRDD[135] at parallelize at cmd56.sc:1\n",
       "\u001b[36msum\u001b[39m: \u001b[32mInt\u001b[39m = \u001b[32m172\u001b[39m"
      ]
     },
     "execution_count": 57,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "// reduce\n",
    "// The reduce() function takes the two elements as input from the RDD and then produces the output of the same type as that of the input elements. The simple forms of such function are an addition. We can add the elements of RDD, count the number of words. It accepts commutative and associative operations as an argument.\n",
    "\n",
    "\n",
    "val rdd1 = spark.sparkContext.parallelize(List(20,32,45,62,8,5))\n",
    "val sum = rdd1.reduce(_+_)\n",
    "println(sum)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 58,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">fold at cmd57.sc:3</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "(total,182)\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mrdd1\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[(\u001b[32mString\u001b[39m, \u001b[32mInt\u001b[39m)] = ParallelCollectionRDD[136] at parallelize at cmd57.sc:1\n",
       "\u001b[36madditionalMarks\u001b[39m: (\u001b[32mString\u001b[39m, \u001b[32mInt\u001b[39m) = (\u001b[32m\"extra\"\u001b[39m, \u001b[32m4\u001b[39m)\n",
       "\u001b[36msum\u001b[39m: (\u001b[32mString\u001b[39m, \u001b[32mInt\u001b[39m) = (\u001b[32m\"total\"\u001b[39m, \u001b[32m182\u001b[39m)"
      ]
     },
     "execution_count": 58,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "// fold()\n",
    "// The signature of the fold() is like reduce(). Besides, it takes “zero value” as input, which is used for the initial call on each partition. But, the condition with zero value is that it should be the identity element of that operation. The key difference between fold() and reduce() is that, reduce() throws an exception for empty collection, but fold() is defined for empty collection.\n",
    "\n",
    "val rdd1 = spark.sparkContext.parallelize(List((\"maths\", 80),(\"science\", 90)))\n",
    "val additionalMarks = (\"extra\", 4)\n",
    "val sum = rdd1.fold(additionalMarks){ (acc, marks) => val add = acc._2 + marks._2\n",
    "(\"total\", add)\n",
    "}\n",
    "println(sum)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 58,
   "metadata": {},
   "outputs": [],
   "source": [
    "// aggregate"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 59,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">parallelize at cmd58.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    3 / 3\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">collect at cmd58.sc:2</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    3 / 3\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "(s,CompactBuffer(3, 4))\n",
      "(p,CompactBuffer(7, 5))\n",
      "(t,CompactBuffer(8))\n",
      "(k,CompactBuffer(5, 6))\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mdata\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[(\u001b[32mChar\u001b[39m, \u001b[32mInt\u001b[39m)] = ParallelCollectionRDD[137] at parallelize at cmd58.sc:1\n",
       "\u001b[36mgroup\u001b[39m: \u001b[32mArray\u001b[39m[(\u001b[32mChar\u001b[39m, \u001b[32mIterable\u001b[39m[\u001b[32mInt\u001b[39m])] = \u001b[33mArray\u001b[39m(\n",
       "  (\u001b[32m's'\u001b[39m, \u001b[33mCompactBuffer\u001b[39m(\u001b[32m3\u001b[39m, \u001b[32m4\u001b[39m)),\n",
       "  (\u001b[32m'p'\u001b[39m, \u001b[33mCompactBuffer\u001b[39m(\u001b[32m7\u001b[39m, \u001b[32m5\u001b[39m)),\n",
       "  (\u001b[32m't'\u001b[39m, \u001b[33mCompactBuffer\u001b[39m(\u001b[32m8\u001b[39m)),\n",
       "  (\u001b[32m'k'\u001b[39m, \u001b[33mCompactBuffer\u001b[39m(\u001b[32m5\u001b[39m, \u001b[32m6\u001b[39m))\n",
       ")"
      ]
     },
     "execution_count": 59,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "// foreach\n",
    "// When we have a situation where we want to apply operation on each element of RDD, but it should not return value to the driver.\n",
    "\n",
    "val data = spark.sparkContext.parallelize(Array(('k',5),('s',3),('s',4),('p',7),('p',5),('t',8),('k',6)),3)\n",
    "val group = data.groupByKey().collect()\n",
    "group.foreach(println)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": 60,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "\u001b[36mres59\u001b[39m: \u001b[32mClass\u001b[39m[\u001b[32mT\u001b[39m] = class org.apache.spark.rdd.ParallelCollectionRDD"
      ]
     },
     "execution_count": 60,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "rdd.getClass"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 61,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">take at cmd60.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    1 / 1\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mres60\u001b[39m: \u001b[32mArray\u001b[39m[\u001b[32mInt\u001b[39m] = \u001b[33mArray\u001b[39m(\u001b[32m1\u001b[39m)"
      ]
     },
     "execution_count": 61,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "rdd.take(1)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 62,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "\u001b[36mdata\u001b[39m: \u001b[32mArray\u001b[39m[\u001b[32mInt\u001b[39m] = \u001b[33mArray\u001b[39m(\u001b[32m1\u001b[39m, \u001b[32m2\u001b[39m, \u001b[32m3\u001b[39m, \u001b[32m4\u001b[39m, \u001b[32m5\u001b[39m)\n",
       "\u001b[36mdistData\u001b[39m: \u001b[32morg\u001b[39m.\u001b[32mapache\u001b[39m.\u001b[32mspark\u001b[39m.\u001b[32mrdd\u001b[39m.\u001b[32mRDD\u001b[39m[\u001b[32mInt\u001b[39m] = ParallelCollectionRDD[139] at parallelize at cmd61.sc:2"
      ]
     },
     "execution_count": 62,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "val data = Array(1, 2, 3, 4, 5)\n",
    "val distData = sc.parallelize(data)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 63,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "  <span style=\"float: left;\">reduce at cmd62.sc:1</span>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/html": [
       "<div class=\"progress\">\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: blue; width: 100%; word-wrap: normal; white-space: nowrap; text-align: center; color: white\" aria-valuenow=\"100\" aria-valuemin=\"0\" aria-valuemax=\"100\">\n",
       "    2 / 2\n",
       "  </div>\n",
       "  <div class=\"progress-bar\" role=\"progressbar\" style=\"background-color: red; width: 0%\" aria-valuenow=\"0\" aria-valuemin=\"0\" aria-valuemax=\"100\"></div>\n",
       "</div>\n"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "data": {
      "text/plain": [
       "\u001b[36mres62\u001b[39m: \u001b[32mInt\u001b[39m = \u001b[32m15\u001b[39m"
      ]
     },
     "execution_count": 63,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "distData.reduce((a, b) => a + b)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 63,
   "metadata": {},
   "outputs": [],
   "source": [
    "// distData.map(s => s.length).reduce((a, b) => a + b)"
   ]
  },
  {
   "cell_type": "raw",
   "metadata": {},
   "source": [
    "RDDs support two types of operations:\n",
    "\n",
    "1. transformations, which create a new dataset from an existing one, map - lazy\n",
    "\n",
    "2. actions, which return a value to the driver program after running a computation on the dataset."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 64,
   "metadata": {},
   "outputs": [],
   "source": [
    "// val lines = sc.textFile(\"data.txt\")"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 64,
   "metadata": {},
   "outputs": [],
   "source": [
    "// val lineLengths = lines.map(s => s.length)\n",
    "// linesLengths"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 64,
   "metadata": {},
   "outputs": [],
   "source": [
    "// val totalLength = lineLengths.reduce((a, b) => a + b)\n",
    "// print(totalLength)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 64,
   "metadata": {},
   "outputs": [],
   "source": [
    "// Working with Key-Value Pairs"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 64,
   "metadata": {},
   "outputs": [],
   "source": [
    "// val pairs = lines.map(s => (s, 1))\n",
    "// val counts = pairs.reduceByKey((a, b) => a + b)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 64,
   "metadata": {},
   "outputs": [],
   "source": [
    "// pairs.take(2)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 64,
   "metadata": {},
   "outputs": [],
   "source": [
    "// val counts = pairs.reduceByKey((a, b) => a + b)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 64,
   "metadata": {},
   "outputs": [],
   "source": [
    "// print(counts.count())"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 64,
   "metadata": {},
   "outputs": [],
   "source": [
    "// lines.sample(withReplacement= true , fraction=0.2, 1 ).take(2)"
   ]
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Scala 2.12",
   "language": "scala",
   "name": "scala212"
  },
  "language_info": {
   "codemirror_mode": "text/x-scala",
   "file_extension": ".scala",
   "mimetype": "text/x-scala",
   "name": "scala",
   "nbconvert_exporter": "script",
   "version": "2.12.10"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 4
}
