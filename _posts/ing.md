# Q1. 파이썬에서는 리스트 형태의 데이터를 자주 사용합니다. 그래서 리스트 를 잘 다루는 것이 중요한데, 다음으로 주어진 리스트 데이터를 다뤄봅시다. 
# 다양한 데이터를 수집해서 아래와 같은 num_list를 얻었습니다. 
# 하지만 우리에게 필요한 데이터는 홀수 데이터입니다. 
# 그렇다면 num_list 가 홀수인 데이터만 출력하도록 하는 함수를 작성해보세요. 

num_list = [1, 5, 7, 15, 16, 22, 28, 29]

def get_odd_num(num_list):

    for i in range(len(num_list)):
      
      if num_list[i] % 2 == 0: continue

      print(num_list[i])

print(get_odd_num(num_list))


# Q2. 데이터 처리를 위해서 문자열을 입력받았습니다. 그런데, 문자열을 받았 더니 단어 단위로 거꾸로 입력되었습니다. 이를 다시 원래대로 출력하는 함수 를 작성해보세요. 
# string 문장을 받아 단어를 역순으로 출력하는 함수를 작성하세요.
# string 연산을 이용해보세요.

sentence = "way a is there will a is there Where"
sentence = sentence.split(' ')

def reverse_sentence(sentence):
    reverse_message = ""

    for i in sentence:
      reverse_message = i + " " +  reverse_message
    print(reverse_message)

print(reverse_sentence(sentence))


# Q3. 이번 학기의 중간고사, 기말고사 점수가 발표되었습니다. 각 학생들의 점 수가 튜플 형태로 저장되어 있고, 이를 포함한 리스트가 있습니다. 이를 이용 해 각 학생들의 평균 점수를 출력하는 함수를 제작하세요. 
# 리스트와 반복문을 사용해 데이터를 불러오세요.
# 이를 이용해 각 학생별 평균을 구해보세요

score = [(100, 100), (95, 90), (55, 60), (75, 80), (70, 70)]

def get_avg(score):
  for i in range(len(score)):

    x = score[i]
    y = (x[0] + x[1]) / 2
    print(f"[{i}] 번, 평균 : {y}")

get_avg(score)


# Q4. 두개의 납품처에서 각각 과일과 야채들이 납품되었습니다. 이를 각각 물 품의 갯수를 나타내는 2개의 딕셔너리로 정리했습니다. 물품을 정리하기 위해서 2 개의 딕셔너리 객체를 합쳐 출력하는 함수를 제작하세요. 
# 중복되는 물품은 합쳐서 표시하세요.
# 각 딕셔너리 데이터의 데이터의 키값을 이용해 중복을 확인해보세요.

dict_first = {'사과': 30, '배': 15, '감': 10, '포도': 10}
dict_second = {'사과': 5, '감': 25, '배': 15, '귤': 25}

def merge_dict(dict_first, dict_second):

  from itertools import chain
  from collections import defaultdict

  dict3 = defaultdict(list)
  for k, v in chain(dict_first.items(), dict_second.items()):
      dict3[k].append(v)

  for k, v in dict3.items():
      print(k, v)

merge_dict(dict_first, dict_second)
    

# Q5. 단어들을 입력받았는데, 자꾸만 숫자들이 섞여들어가는 문제가 있습니 다. 이를 처리하기 위해서 함수에 string을 전달 받은 뒤, string 안에서의 숫 자를 제거한 후 string만 남은 리스트를 출력하세요. 😎
#string 연산을 이용해서 문자열을 자르는 연산을 사용해보세요.

inputs = "cat32dog16cow5"

def find_string(inputs):
  
  import re
  x = re.compile(r'-?\d+\.?\d*').sub(' ', inputs)
  y = x.split()
  print(y)

string_list = find_string(inputs)
print(string_list)