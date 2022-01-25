# Q1. 중간고사 기말고사 점수를 따로 받아 저장하는 클래스를 구현해보세요. 단, 생성자의 인스턴스는 private으로 선언되어야하며, 데코레이터를 이용해 데이터를 저장하고, 함수를 이용해 평균값을 출력해보세요.
# - 자료형의 선언과 데코레이터를 이용해보세요.

class Score(object):
    def __init__(self, mid, final):
        self.__mid = mid
        self.__final = final

    @property
    def mid(self):
        return self.__mid
    
    @property
    def final(self):
        return self.__final         

score = Score(50, 75)
print((score.mid + score.final)/2)

# Q2. 다양한 탈 것을 사용하는 게임을 만드는 중입니다. 빠른 구현을 위해서 이미 구현한 Car 클래스를 이용해서 Bike라는 클래스를 새로 제작하려고 합니다. Car 클래스를 상속받아서 새로운 Bike 클래스를 아래와 같이 출력되도록 구성해보세요.
#  Bike class는 size 인자를 추가로 가집니다.

class Car():
  def __init__(self, fuel, wheels):
    self.fuel = fuel
    self.wheels = wheels
  
class Bike(Car):
  def __init__(self, fuel, wheels, size):
    super().__init__(fuel, wheels)
    self.size = size

bike = Bike("gas", 2, "small")
print(bike.fuel, bike.wheels, bike.size)

# Q3. 이번 시험 결과에 대한 데이터를 학과 사무실에서 CSV파일로 전달해줬습니다. 우리는 이 파일을 이용해서 데이터 처리를 진행해야 합니다. 파일 입출력을 이용해 파일 데이터를 리스트로 만들어보세요.
# - 파일 입출력에 사용하는 open 함수를 이용해 CSV 파일 내부의 데이터를 읽어보세요

# file_path = './data-01-test-score.csv'

# import csv

# def read_file(file_path):
#     with open(file_path, "r", encoding="utf-8") as f:
#         f_list = csv.reader(f) 
#         result = []
#         for i in f_list:
#           result.append(i)
#         f.close()  
#     return result      
           
# print(read_file(file_path))


file_path = './data-01-test-score.csv'

import csv

def read_file(file_path):

  with open(file_path, "r", encoding="utf-8") as f:
      f_list = csv.reader(f) 
      result = [i for i in f_list]
  return result      

print(read_file(file_path))


# Q4. 우리는 방금 CSV 파일을 읽는 함수를 구현해보았습니다. 하지만 이를 조금 더 효율적으로 사용하기 위해서 클래스로 구성을 진행하려고 합니다. 방금 구현한 함수를 포함한 클래스를 구현해보세요.
#  - merge list를 이용해 리스트 내 데이터의 합을 출력해보세요.
# - 데이터를 합치기 전 데이터의 자료형을 변경해보세요.


file_path = './data-01-test-score.csv'

import csv

class ReadCSV(object):
  def __init__(self, file_path):
    self.file_path = file_path

  def read_file(self, file_path):
    with open(file_path, "r", encoding="utf-8") as f:
      f_list = csv.reader(f) 
      result = [i for i in f_list]
      global r_result
      r_result = []
      for j in result:
          r_result.append(list(map(int,j)))
      f.close() 
    return r_result      

  def merge_list(self, file_path):
    sum_list = []
    for i in range(len(r_result)):
      sum_list.append(sum(r_result[i]))
    return sum_list

read_csv = ReadCSV(file_path)
print(read_csv.read_file(file_path))
print(read_csv.merge_list(file_path))


# Q5. 이전에 구현한 클래스에서 merge_list의 함수 동작을 변경해야합니다. 단순합계가 아닌 평균을 구하는 함수로 변경해보세요.
#  - 리스트의 데이터를 다루는 함수를 이용해서 구현해보세요.
# - 최종 평균을 구한 후 오름차순으로 정렬해주세요.

file_path = './data-01-test-score.csv'

import csv

class ReadCSV(object):
  def __init__(self, file_path):
    self.file_path = file_path

  def read_file(self, file_path):
    with open(file_path, "r", encoding="utf-8") as f:
      f_list = csv.reader(f) 
      result = [i for i in f_list]
      global r_result
      r_result = []
      for j in result:
          r_result.append(list(map(int,j)))
      f.close() 
    return r_result      

  def merge_list(self, file_path):
    avg_list = []
    for i in range(len(r_result)):
      avg_list.append(sum(r_result[i]) / len(r_result[i]))
      avg_list.sort()  
    return avg_list

read_csv = ReadCSV(file_path)
print(read_csv.merge_list(read_csv.read_file(file_path)))