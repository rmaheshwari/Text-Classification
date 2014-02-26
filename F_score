from __future__ import division
import ast
import math
import sys
import os
import time

start_time = time.time()

fileCount = {}
classifiedFileCount = {}
correctClassifiedFileCount = {}

f = open('spam.nb', 'r')
vocabSize = int(f.readline())
pClasses = ast.literal_eval(f.readline())
pWords = ast.literal_eval(f.readline())
pZeroCount = ast.literal_eval(f.readline())

pClassGvnMsg = {}
directory = "../../../../NLP/SPAM_dev/"
for subdir, dirs, files in os.walk(directory):
    for testFile in files:
        if not testFile.startswith('.'):
            for className in pClasses:
                pClassGvnMsg[className] = math.log(pClasses[className])

            testF = open(directory + testFile, 'r')
            lines = testF.readlines()

            for line in lines:
                words = line.split(' ')
                for word in words:
                    word = word.strip()
                    if word != '':
                        if not word in pWords:
                            pMap = pZeroCount
                        else:
                            pMap = pWords[word]

                    for eachClass in pClassGvnMsg:
                        pClassGvnMsg[eachClass] += math.log(pMap[eachClass])
                    else:
                        continue

            maxProbability = (-sys.maxint - 1)
            finalMsgClass = ''
            for eachClass in pClassGvnMsg:
                if maxProbability <= pClassGvnMsg[eachClass]:
                    finalMsgClass = eachClass
                    maxProbability = pClassGvnMsg[eachClass]

            actualClass = testFile.split('.')[0]
            if actualClass in fileCount:
                fileCount[actualClass] += 1
            else:
                fileCount[actualClass] = 1

            if finalMsgClass in classifiedFileCount:
                classifiedFileCount[finalMsgClass] += 1
            else:
                classifiedFileCount[finalMsgClass] = 1

            if finalMsgClass == actualClass:
                if finalMsgClass in correctClassifiedFileCount:
                    correctClassifiedFileCount[finalMsgClass] += 1
                else:
                    correctClassifiedFileCount[finalMsgClass] = 1

for eachClass in fileCount:
    precision = correctClassifiedFileCount[eachClass] / classifiedFileCount[eachClass]
    recall = correctClassifiedFileCount[eachClass] / fileCount[eachClass]
    fScore = (2 * precision * recall) / (precision + recall)

    print eachClass + '-->' + str(precision) + '--' + str(recall) + '--' + str(fScore)

print time.time() - start_time, "seconds"
