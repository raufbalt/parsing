from peewee import *
import requests
from bs4 import BeautifulSoup
import datetime
import psycopg2
from psycopg2 import Error



""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

now = datetime.datetime.now()

page = 1

title = ''
price = ''
date = ''
image = ''

while True:

    url = ('https://www.kijiji.ca/b-apartments-condos/city-of-toronto/c37l1700273' + str(page))

    rock = requests.get(url)

    soup = BeautifulSoup(rock.text, "html.parser")

    teme = soup.find_all('div', class_='info')
    teme_image = soup.find_all('div', class_='image')

    for tem in teme:

        title = (tem.find('div', {'class': 'title'}).text.strip())
        price = (tem.find('div', {'class': 'price'}).text.strip())
        date = (tem.find('span', {'class': 'date-posted'}).text.strip())

    for te in teme_image:
        try:
            image = te.find('source').get('data-srcset')
        except AttributeError:
            image = 'None'

        if "/" not in date:
            date = now.strftime("%d/%m/%Y")

        page += 1

    break

""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

try:
    # Подключиться к существующей базе данных
    connection = psycopg2.connect(user="rauf",
                                  # пароль, который указали при установке PostgreSQL
                                  password="1",
                                  host="127.0.0.1",
                                  port="5432",
                                  database="parsing_db")

    # Создайте курсор для выполнения операций с базой данных
    cursor = connection.cursor()
    # SQL-запрос для создания новой таблицы
    # create_table_query = '''CREATE TABLE item (
	#     item_title VARCHAR (100) NOT NULL,
    # 	date VARCHAR (100) NOT NULL,
	#     price VARCHAR (100) NOT NULL,
    #     image VARCHAR (100) NOT NULL
    # );'''
    # # Выполнение команды: это создает новую таблицу
    # cursor.execute(create_table_query)
    # connection.commit()
    # print("Таблица успешно создана в PostgreSQL")


    insert_query = """ INSERT INTO item (item_title, date, price, image)
                              VALUES ('{title}', '{date}', '{price}', '{image}')"""
    cursor.execute(insert_query)
    connection.commit()
    print("1 элемент успешно добавлен")

except (Exception, Error) as error:
    print("Ошибка при работе с PostgreSQL", error)
finally:
    if connection:
        cursor.close()
        connection.close()
        print("Соединение с PostgreSQL закрыто")
