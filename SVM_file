import time
import os

start_time = time.time()
labelAndContent = {}
feature_id = {}
id_feature = {}
each_word_count = {}
label_id = {}
id_label = {}
last_feature_id = 1
last_label_id = -3

f = open('sentiment_training.txt', 'r')
lines = f.readlines()
f.close()

for line in lines:
    words = line.split(' ')
    class_name = ''
    for index, word in enumerate(words):
        word = word.strip()

        if (index == 0) and (word != ''):
            class_name = word
            if not word in label_id:
                last_label_id += 2
                label_id[word] = last_label_id
                id_label[last_label_id] = word

        if (index != 0) and (word != '') and (not word in each_word_count):
            last_feature_id += 1
            feature_id[word] = last_feature_id
            id_feature[last_feature_id] = word
            each_word_count[word] = 1
        elif (index != 0) and (word != ''):
            each_word_count[word] += 1

    if class_name != '':
        if not class_name in labelAndContent:
            labelAndContent[class_name] = [line]
        else:
            contentList = labelAndContent[class_name]
            contentList.append(line)

print "Writing"
f = open('sentiment_training_svm.txt', 'w')
for each_label in labelAndContent:
    contentList = labelAndContent[each_label]
    for contentLine in contentList:
        words_in_line = contentLine.split(' ')
        words_id_list = []
        for wIndex, each_word in enumerate(words_in_line):
            if wIndex == 0:
                continue

            each_word = each_word.strip()
            if each_word == '':
                continue

            words_id_list.append(feature_id[each_word])

        words_id_list = sorted(set(words_id_list))
        final_op_line = ''
        for each_word_id in words_id_list:
            final_op_line += str(each_word_id) + ':' + str(each_word_count[id_feature[each_word_id]]) + ' '

        f.write(str(label_id[each_label]) + ' ' + final_op_line + '\n')
f.close()


print "Writing test"
directory = "../../../../NLP/SENTIMENT_test/"
contentMap = {}

for subdir, dirs, files in os.walk(directory):
    for myFile in files:
        if not myFile.startswith('.'):
            f = open(directory + myFile)
            lines = f.readlines()
            f.close()

            singleLine = ''
            for line in lines:
                line = line.replace('\n\r', ' ')
                line = line.replace('\r\n', ' ')
                line = line.replace('\n', ' ')
                line = line.replace('\r', ' ')
                line = line.replace('<br />', '')
                singleLine += line

            if singleLine != '':
                label = myFile.split('.')[0].strip()
                this_label_id = 0
                if label in label_id:
                    this_label_id = label_id[label]

                if this_label_id in contentMap:
                    contentList = contentMap[this_label_id]
                    contentList.append(singleLine)
                else:
                    contentMap[this_label_id] = [singleLine]

f = open('sentiment_test_svm.txt', 'w')
for each_label_id in contentMap:
    all_lines = contentMap[each_label_id]
    for each_line in all_lines:
        all_words = each_line.split(' ')
        all_word_ids = []

        for each_word in all_words:
            each_word = each_word.strip()

            if each_word == '':
                continue

            this_word_id = 0
            if each_word in feature_id:
                this_word_id = feature_id[each_word]
            all_word_ids.append(this_word_id)

        all_word_ids = sorted(set(all_word_ids))
        final_op_line = ''
        for each_unique_word_id in all_word_ids:
            if each_unique_word_id != 0:
                final_op_line += str(each_unique_word_id) + ':' + str(each_word_count[id_feature[each_unique_word_id]])+ ' '

        f.write(str(each_label_id) + ' ' + final_op_line + '\n')


print time.time() - start_time, "seconds"
