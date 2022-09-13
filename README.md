# dobor-2-zadacha
import requests
import os
import threading
import json


def download(threads):

     if not os.path.isdir("user_data"):
          os.mkdir("user_data")


     current_way = os.getcwd() + '\\user_data\\'

     for i in range(12): #Возьмём 12 случайных людей( по API ) рандомно выпадают.
          user_data = json.loads(requests.get('https://randomuser.me/api/').text) #словарь с человеком 
          cat = json.loads(requests.get('https://catfact.ninja/fact').text)

          print(cat)

          image = user_data['results'][0]['picture']['large']
          image = requests.get(image).content #ссылка на фотографию пользователя.
          cur_new = current_way + str(i+1)
          file_way = os.getcwd() + '\\user_data\\' + str(i+1)
          if not os.path.isdir(cur_new):
               os.mkdir(cur_new)

          direct = cur_new + "\\user_information.txt"
          photo_dir = cur_new + "\\photo.png"
          cat_dict = cur_new + "\\info_from_cat.txt"

          with open(direct, 'a+', encoding='utf-8') as f:

               f.write(f'Surname: {user_data["results"][0]["name"]["first"]}, Surname: {user_data["results"][0]["name"]["last"]}')

          with open(cat_dict, 'a+', encoding='utf-8') as catttt:
               catttt.write(f'Funny or Not fact from CAT: {cat["fact"]}')

          with open(photo_dir, 'wb') as photo:
               photo.write(image)

thread_list= []
for i in range(1,5):
     thr = threading.Thread(target=download, args=(i,))
     thr.start()
     thread_list.append(thr)

for thread in thread_list:
     thread.join()
