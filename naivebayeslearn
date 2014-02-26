from __future__ import division
import time
import sys
import ast


def main():
    """
    Main function
    """
    if len(sys.argv) != 3:
        print "Error: Insufficient number of arguments"
        exit()

    cmd_args = ast.literal_eval(str(sys.argv))

    vocab = {}
    word_count_for_each_class = {}
    each_class_word_count = {}
    classes = {}

    f = open(cmd_args[1])
    lines = f.readlines()
    f.close()

    for line in lines:
        words = line.split(' ')
        class_name = ''
        no_of_words = 0
        for index, word in enumerate(words):
            word = word.strip()

            if (index == 0) and (word != ''):
                class_name = word
                if not class_name in classes:
                    classes[class_name] = 1
                else:
                    classes[class_name] += 1

            if (index != 0) and (word != '') and (not word in vocab):
                vocab[word] = 1
                class_word_count = {class_name: 1}
                word_count_for_each_class[word] = class_word_count
                no_of_words += 1
            elif (index != 0) and (word != ''):
                class_word_count = word_count_for_each_class[word]
                if not class_name in class_word_count:
                    class_word_count[class_name] = 1
                else:
                    class_word_count[class_name] += 1
                no_of_words += 1

        if (class_name != '') and (not class_name in each_class_word_count):
            each_class_word_count[class_name] = no_of_words
        elif class_name != '':
            each_class_word_count[class_name] += no_of_words

    vocab_size = len(vocab)
    p_classes = {}
    p_words = {}
    p_zero_count = {}
    total_class_count = 0
    for thisClassCount in classes.values():
        total_class_count += thisClassCount

    for eachClassName in classes.keys():
        p_classes[eachClassName] = (classes[eachClassName] / total_class_count)
        p_zero_count[eachClassName] = 1 / (each_class_word_count[eachClassName] + vocab_size)

    for eachWord in word_count_for_each_class.keys():
        for eachClassName in classes.keys():
            denominator = each_class_word_count[eachClassName] + vocab_size
            if eachClassName in word_count_for_each_class[eachWord]:
                probability = (word_count_for_each_class[eachWord][eachClassName] + 1) / denominator
            else:
                probability = 1 / denominator

            if eachWord in p_words:
                p_words[eachWord][eachClassName] = probability
            else:
                p_words[eachWord] = {eachClassName: probability}

    f = open(cmd_args[2], 'w')
    f.write(str(vocab_size))
    f.write('\n')
    f.write(str(p_classes))
    f.write('\n')
    f.write(str(p_words))
    f.write('\n')
    f.write(str(p_zero_count))
    f.close()


start_time = time.time()
main()
print "Done in", (time.time() - start_time), "seconds"
