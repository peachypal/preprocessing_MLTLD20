Step 0. Importing libraries...


```python
import sqlite3
import csv
import pandas as pd
```

Step 1. Creating a database and merging two tables


I'll be working with two tables that I have created and worked with during my practicum. 

The first table contains the information about the participants I gathered. The table consists of five columns which include the participants' IDs, their age, gender, and level of education. Note that there are two columns created for their level of education: the category (e.g. university degree, secondary vocational, etc) and its corresponding group. So, for 'incomplete secondary vocational' it would be group 1, for 'secondary vocational' it's group 2, for 'incomplete university degree' it's 3, for 'university degree' it's 4, and finally for 'phd' it's 5.
Also, we used 0 and 1 for genders, where 0 corresponds to 'male' and 1 corresponds to 'female'. 

The second table contains the answers that my participants have provided for one task from the experiment. The task was to explain the meaning of an utterance that uses a metaphor, a comparison, or a proverb, and was aimed to check whether the participant understands the pragmatic meaning of the said utterance. This table consists of five columns, as well: those are participants' IDs, the code name of the task ('figurative 2'), the task items (1-15), the participants' score (0-2, where 0 is literal interpretation/rephrasing of the utterance/wrong answer/NA, 1 is partially correct (e.g. the participant uses a concrete example to illustrate their understanding of an utterance but fails to provide a general interpretation), and 2 is correct), and the participants' answers in Russian.

I've decided to combine these two tables in a single database using the pd.merge() function.
First I'll recreate these two tables, and then I'll merge them into one dataframe. 


```python
# table 1
df1 = pd.read_csv(r'C:\Users\Kaz\Documents\preprocessing python\hw2\p_info.csv', sep=',')
```


```python
# let's check if everything works fine
df1.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Id</th>
      <th>Age</th>
      <th>Gender</th>
      <th>EducGroup</th>
      <th>EducGroupCat</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>101-APACSadd</td>
      <td>21</td>
      <td>1</td>
      <td>4</td>
      <td>university degree</td>
    </tr>
    <tr>
      <th>1</th>
      <td>102-APACSadd</td>
      <td>37</td>
      <td>0</td>
      <td>5</td>
      <td>phd</td>
    </tr>
    <tr>
      <th>2</th>
      <td>103-APACSadd</td>
      <td>21</td>
      <td>0</td>
      <td>4</td>
      <td>university degree</td>
    </tr>
    <tr>
      <th>3</th>
      <td>104-APACSadd</td>
      <td>50</td>
      <td>1</td>
      <td>4</td>
      <td>university degree</td>
    </tr>
    <tr>
      <th>4</th>
      <td>105-APACSadd</td>
      <td>53</td>
      <td>0</td>
      <td>4</td>
      <td>university degree</td>
    </tr>
  </tbody>
</table>
</div>




```python
# table 2
df2 = pd.read_csv(r'C:\Users\Kaz\Documents\preprocessing python\hw2\p_task.csv', sep=',', encoding = "utf-8")
```


```python
df2.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Id</th>
      <th>Task</th>
      <th>Item</th>
      <th>Score</th>
      <th>Answer</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>101-APACSadd</td>
      <td>figurative 2</td>
      <td>1</td>
      <td>2</td>
      <td>не придал значение</td>
    </tr>
    <tr>
      <th>1</th>
      <td>101-APACSadd</td>
      <td>figurative 2</td>
      <td>2</td>
      <td>2</td>
      <td>нет денег</td>
    </tr>
    <tr>
      <th>2</th>
      <td>101-APACSadd</td>
      <td>figurative 2</td>
      <td>3</td>
      <td>2</td>
      <td>рассеянный</td>
    </tr>
    <tr>
      <th>3</th>
      <td>101-APACSadd</td>
      <td>figurative 2</td>
      <td>4</td>
      <td>2</td>
      <td>счастлив</td>
    </tr>
    <tr>
      <th>4</th>
      <td>101-APACSadd</td>
      <td>figurative 2</td>
      <td>5</td>
      <td>2</td>
      <td>пожалеет</td>
    </tr>
  </tbody>
</table>
</div>




```python
# merging the tables into one
# I'll be using a one-to-one join since I think it's important for the database to contain each answer separately.
df = pd.merge(df1, df2)
```


```python
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Id</th>
      <th>Age</th>
      <th>Gender</th>
      <th>EducGroup</th>
      <th>EducGroupCat</th>
      <th>Task</th>
      <th>Item</th>
      <th>Score</th>
      <th>Answer</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>101-APACSadd</td>
      <td>21</td>
      <td>1</td>
      <td>4</td>
      <td>university degree</td>
      <td>figurative 2</td>
      <td>1</td>
      <td>2</td>
      <td>не придал значение</td>
    </tr>
    <tr>
      <th>1</th>
      <td>101-APACSadd</td>
      <td>21</td>
      <td>1</td>
      <td>4</td>
      <td>university degree</td>
      <td>figurative 2</td>
      <td>2</td>
      <td>2</td>
      <td>нет денег</td>
    </tr>
    <tr>
      <th>2</th>
      <td>101-APACSadd</td>
      <td>21</td>
      <td>1</td>
      <td>4</td>
      <td>university degree</td>
      <td>figurative 2</td>
      <td>3</td>
      <td>2</td>
      <td>рассеянный</td>
    </tr>
    <tr>
      <th>3</th>
      <td>101-APACSadd</td>
      <td>21</td>
      <td>1</td>
      <td>4</td>
      <td>university degree</td>
      <td>figurative 2</td>
      <td>4</td>
      <td>2</td>
      <td>счастлив</td>
    </tr>
    <tr>
      <th>4</th>
      <td>101-APACSadd</td>
      <td>21</td>
      <td>1</td>
      <td>4</td>
      <td>university degree</td>
      <td>figurative 2</td>
      <td>5</td>
      <td>2</td>
      <td>пожалеет</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>355</th>
      <td>124-APACSadd</td>
      <td>73</td>
      <td>1</td>
      <td>4</td>
      <td>university degree</td>
      <td>figurative 2</td>
      <td>11</td>
      <td>2</td>
      <td>вас могут всегда услышать</td>
    </tr>
    <tr>
      <th>356</th>
      <td>124-APACSadd</td>
      <td>73</td>
      <td>1</td>
      <td>4</td>
      <td>university degree</td>
      <td>figurative 2</td>
      <td>12</td>
      <td>2</td>
      <td>у любого человека иногда не бывает того, что о...</td>
    </tr>
    <tr>
      <th>357</th>
      <td>124-APACSadd</td>
      <td>73</td>
      <td>1</td>
      <td>4</td>
      <td>university degree</td>
      <td>figurative 2</td>
      <td>13</td>
      <td>2</td>
      <td>что компания одного опоздавшего не ждет</td>
    </tr>
    <tr>
      <th>358</th>
      <td>124-APACSadd</td>
      <td>73</td>
      <td>1</td>
      <td>4</td>
      <td>university degree</td>
      <td>figurative 2</td>
      <td>14</td>
      <td>2</td>
      <td>если на тебя свалилась одна беда, жди другую</td>
    </tr>
    <tr>
      <th>359</th>
      <td>124-APACSadd</td>
      <td>73</td>
      <td>1</td>
      <td>4</td>
      <td>university degree</td>
      <td>figurative 2</td>
      <td>15</td>
      <td>2</td>
      <td>когда есть что-то хорошее, обязательно бывает ...</td>
    </tr>
  </tbody>
</table>
<p>360 rows × 9 columns</p>
</div>



Alright, it seems that everything is working well so far. Now let's make a database out of this table and create a cursor for it.


```python
conn = sqlite3.connect("pdata.db")
cur=conn.cursor()
df.to_sql(name='pdata', con=con, if_exists='replace')
```


```python
for row in cur.execute('SELECT * FROM pdata'):
    print(row)
```

    (0, '101-APACSadd', 21, 1, 4, 'university degree', 'figurative 2', 1, 2, 'не придал значение')
    (1, '101-APACSadd', 21, 1, 4, 'university degree', 'figurative 2', 2, 2, 'нет денег')
    (2, '101-APACSadd', 21, 1, 4, 'university degree', 'figurative 2', 3, 2, 'рассеянный')
    (3, '101-APACSadd', 21, 1, 4, 'university degree', 'figurative 2', 4, 2, 'счастлив')
    (4, '101-APACSadd', 21, 1, 4, 'university degree', 'figurative 2', 5, 2, 'пожалеет')
    (5, '101-APACSadd', 21, 1, 4, 'university degree', 'figurative 2', 6, 2, 'очень тяжелые')
    (6, '101-APACSadd', 21, 1, 4, 'university degree', 'figurative 2', 7, 2, 'причиняют боль, болезненные')
    (7, '101-APACSadd', 21, 1, 4, 'university degree', 'figurative 2', 8, 0, 'не слышала')
    (8, '101-APACSadd', 21, 1, 4, 'university degree', 'figurative 2', 9, 2, 'очень красивые')
    (9, '101-APACSadd', 21, 1, 4, 'university degree', 'figurative 2', 10, 1, 'не расчешешь')
    (10, '101-APACSadd', 21, 1, 4, 'university degree', 'figurative 2', 11, 2, 'вас везде могут подслушать')
    (11, '101-APACSadd', 21, 1, 4, 'university degree', 'figurative 2', 12, 2, 'обслуживает всех кроме себя')
    (12, '101-APACSadd', 21, 1, 4, 'university degree', 'figurative 2', 13, 2, 'общее мнение важнее мнения личности')
    (13, '101-APACSadd', 21, 1, 4, 'university degree', 'figurative 2', 14, 2, 'за одним несчастьем последует второе')
    (14, '101-APACSadd', 21, 1, 4, 'university degree', 'figurative 2', 15, 2, 'в чем-то хорошем всегда найдется что-то плохое')
    (15, '102-APACSadd', 37, 0, 5, 'phd', 'figurative 2', 1, 1, 'не стал принимать меры; не стал действовать, наказывать нарушителя')
    (16, '102-APACSadd', 37, 0, 5, 'phd', 'figurative 2', 2, 2, 'нет денег постоянно')
    (17, '102-APACSadd', 37, 0, 5, 'phd', 'figurative 2', 3, 2, 'не заинтересован в учебе, находится в своем фантазийном мире, не умеет сконцентрироваться')
    (18, '102-APACSadd', 37, 0, 5, 'phd', 'figurative 2', 4, 2, 'вася счастлив')
    (19, '102-APACSadd', 37, 0, 5, 'phd', 'figurative 2', 5, 2, 'марину ждет разочарование')
    (20, '102-APACSadd', 37, 0, 5, 'phd', 'figurative 2', 6, 2, 'очень тяжелые')
    (21, '102-APACSadd', 37, 0, 5, 'phd', 'figurative 2', 7, 2, 'неприятные воспоминания, которые трудно забыть')
    (22, '102-APACSadd', 37, 0, 5, 'phd', 'figurative 2', 8, 2, 'громкие, звонкие, низкие')
    (23, '102-APACSadd', 37, 0, 5, 'phd', 'figurative 2', 9, 2, 'неестественно красивые, без изъянов')
    (24, '102-APACSadd', 37, 0, 5, 'phd', 'figurative 2', 10, 2, 'неопрятные')
    (25, '102-APACSadd', 37, 0, 5, 'phd', 'figurative 2', 11, 2, 'не надо болтать лишнего;  кто-то может услышать')
    (26, '102-APACSadd', 37, 0, 5, 'phd', 'figurative 2', 12, 2, 'человек, который занимается конкретным делом, не может применить это дело к себе')
    (27, '102-APACSadd', 37, 0, 5, 'phd', 'figurative 2', 13, 2, 'большинство решает; демократия; некрасиво заставлять ждать; неэффективно, когда семь человек ждут одного, малое должно подстраиваться под большее, большее управляет малым')
    (28, '102-APACSadd', 37, 0, 5, 'phd', 'figurative 2', 14, 2, 'говорят, когда подряд произошло несколько негативных событий, однако это все чистая случайность')
    (29, '102-APACSadd', 37, 0, 5, 'phd', 'figurative 2', 15, 2, 'непременный негативный атрибут, присутствующий при каком-либо значимом положительном')
    (30, '103-APACSadd', 21, 0, 4, 'university degree', 'figurative 2', 1, 2, 'не стал обращать внимание на то, как она себя ведет')
    (31, '103-APACSadd', 21, 0, 4, 'university degree', 'figurative 2', 2, 2, 'у него нет денег')
    (32, '103-APACSadd', 21, 0, 4, 'university degree', 'figurative 2', 3, 2, 'отвлекается все время')
    (33, '103-APACSadd', 21, 0, 4, 'university degree', 'figurative 2', 4, 2, 'очень счастлив')
    (34, '103-APACSadd', 21, 0, 4, 'university degree', 'figurative 2', 5, 2, 'будет недовольна')
    (35, '103-APACSadd', 21, 0, 4, 'university degree', 'figurative 2', 6, 2, 'тяжелые сумки')
    (36, '103-APACSadd', 21, 0, 4, 'university degree', 'figurative 2', 7, 2, 'их сложно забыть')
    (37, '103-APACSadd', 21, 0, 4, 'university degree', 'figurative 2', 8, 2, 'очень громкие голоса')
    (38, '103-APACSadd', 21, 0, 4, 'university degree', 'figurative 2', 9, 2, 'очень красивые манекенщицы')
    (39, '103-APACSadd', 21, 0, 4, 'university degree', 'figurative 2', 10, 2, 'очень густые прически')
    (40, '103-APACSadd', 21, 0, 4, 'university degree', 'figurative 2', 11, 2, 'надо аккуратно разговаривать')
    (41, '103-APACSadd', 21, 0, 4, 'university degree', 'figurative 2', 12, 0, 'человек, который нарушает ожидание; любой человек ожидает чего-то одного, а он этого не делает или не умеет; есть какое-то разочарование')
    (42, '103-APACSadd', 21, 0, 4, 'university degree', 'figurative 2', 13, 2, 'мнение большинства важнее мнения одного')
    (43, '103-APACSadd', 21, 0, 4, 'university degree', 'figurative 2', 14, 2, 'если что-то плохо, то что-то тоже будет плохо')
    (44, '103-APACSadd', 21, 0, 4, 'university degree', 'figurative 2', 15, 2, 'плохая мелочь, которая портит всю картину')
    (45, '104-APACSadd', 50, 1, 4, 'university degree', 'figurative 2', 1, 2, 'не обратил внимание на ее поведение')
    (46, '104-APACSadd', 50, 1, 4, 'university degree', 'figurative 2', 2, 2, 'у моего брата нету денег')
    (47, '104-APACSadd', 50, 1, 4, 'university degree', 'figurative 2', 3, 2, 'этот ученик отвлекается')
    (48, '104-APACSadd', 50, 1, 4, 'university degree', 'figurative 2', 4, 2, 'очень рад')
    (49, '104-APACSadd', 50, 1, 4, 'university degree', 'figurative 2', 5, 2, 'потом будет жалеть')
    (50, '104-APACSadd', 50, 1, 4, 'university degree', 'figurative 2', 6, 2, 'очень тяжелые')
    (51, '104-APACSadd', 50, 1, 4, 'university degree', 'figurative 2', 7, 2, 'невозможно забыть, очень болезненные')
    (52, '104-APACSadd', 50, 1, 4, 'university degree', 'figurative 2', 8, 2, 'очень громкие')
    (53, '104-APACSadd', 50, 1, 4, 'university degree', 'figurative 2', 9, 2, 'красивые')
    (54, '104-APACSadd', 50, 1, 4, 'university degree', 'figurative 2', 10, 2, 'неопрятные')
    (55, '104-APACSadd', 50, 1, 4, 'university degree', 'figurative 2', 11, 2, 'кто-то подслушивает')
    (56, '104-APACSadd', 50, 1, 4, 'university degree', 'figurative 2', 12, 2, 'чужим что-то делает, а у самого нету')
    (57, '104-APACSadd', 50, 1, 4, 'university degree', 'figurative 2', 13, 2, 'большинство не ждет меньшинство, одного')
    (58, '104-APACSadd', 50, 1, 4, 'university degree', 'figurative 2', 14, 2, 'бывает череда неприятностей')
    (59, '104-APACSadd', 50, 1, 4, 'university degree', 'figurative 2', 15, 1, 'добавил что-то, что обесценил, все испортил\n')
    (60, '105-APACSadd', 53, 0, 4, 'university degree', 'figurative 2', 1, 1, 'не обратил внимание, что у нее нет билета')
    (61, '105-APACSadd', 53, 0, 4, 'university degree', 'figurative 2', 2, 2, 'всегда нет денег')
    (62, '105-APACSadd', 53, 0, 4, 'university degree', 'figurative 2', 3, 1, 'невнимательно слушает урок')
    (63, '105-APACSadd', 53, 0, 4, 'university degree', 'figurative 2', 4, 2, 'счастлив от того, что его повысили на работе')
    (64, '105-APACSadd', 53, 0, 4, 'university degree', 'figurative 2', 5, 2, 'марина будет очень жалеть')
    (65, '105-APACSadd', 53, 0, 4, 'university degree', 'figurative 2', 6, 2, 'очень тяжелые')
    (66, '105-APACSadd', 53, 0, 4, 'university degree', 'figurative 2', 7, 2, 'очень неприятны')
    (67, '105-APACSadd', 53, 0, 4, 'university degree', 'figurative 2', 8, 2, 'громкие, зычные голоса')
    (68, '105-APACSadd', 53, 0, 4, 'university degree', 'figurative 2', 9, 2, 'очень красивые манекенщицы')
    (69, '105-APACSadd', 53, 0, 4, 'university degree', 'figurative 2', 10, 2, 'очень растрепанные, густые')
    (70, '105-APACSadd', 53, 0, 4, 'university degree', 'figurative 2', 11, 2, 'вас могут подслушать даже в закрытом, пустом помещении')
    (71, '105-APACSadd', 53, 0, 4, 'university degree', 'figurative 2', 12, 2, 'профессионал часто оказывается не в состоянии помочь самому себе')
    (72, '105-APACSadd', 53, 0, 4, 'university degree', 'figurative 2', 13, 2, 'всегда нужно приходить вовремя')
    (73, '105-APACSadd', 53, 0, 4, 'university degree', 'figurative 2', 14, 2, 'очень часто неприятности следуют одна за другой')
    (74, '105-APACSadd', 53, 0, 4, 'university degree', 'figurative 2', 15, 2, 'не бывает, чтобы все было абсолютно точно хорошо')
    (75, '106-APACSadd', 42, 1, 5, 'phd', 'figurative 2', 1, 2, 'перестал обращать внимание')
    (76, '106-APACSadd', 42, 1, 5, 'phd', 'figurative 2', 2, 2, 'у него нет денег')
    (77, '106-APACSadd', 42, 1, 5, 'phd', 'figurative 2', 3, 2, 'мечтает, не сосредотачивается')
    (78, '106-APACSadd', 42, 1, 5, 'phd', 'figurative 2', 4, 2, 'счастлив')
    (79, '106-APACSadd', 42, 1, 5, 'phd', 'figurative 2', 5, 2, 'сожалеть')
    (80, '106-APACSadd', 42, 1, 5, 'phd', 'figurative 2', 6, 2, 'очень тяжелые')
    (81, '106-APACSadd', 42, 1, 5, 'phd', 'figurative 2', 7, 2, 'неприятное воспоминание, которое тяжело забыть')
    (82, '106-APACSadd', 42, 1, 5, 'phd', 'figurative 2', 8, 2, 'очень громкие')
    (83, '106-APACSadd', 42, 1, 5, 'phd', 'figurative 2', 9, 2, 'очень красивые')
    (84, '106-APACSadd', 42, 1, 5, 'phd', 'figurative 2', 10, 2, 'нестриженые, неаккуратные')
    (85, '106-APACSadd', 42, 1, 5, 'phd', 'figurative 2', 11, 2, 'слышат все, подслушивают')
    (86, '106-APACSadd', 42, 1, 5, 'phd', 'figurative 2', 12, 0, 'не умеет делать что-то, как правило, свою работу')
    (87, '106-APACSadd', 42, 1, 5, 'phd', 'figurative 2', 13, 2, 'много людей не ждет одного')
    (88, '106-APACSadd', 42, 1, 5, 'phd', 'figurative 2', 14, 2, 'если что-то случилось, как правило будет несколько неприятностей')
    (89, '106-APACSadd', 42, 1, 5, 'phd', 'figurative 2', 15, 2, 'все хорошо, но что-то малое портит впечатление')
    (90, '107-APACSadd', 37, 1, 4, 'university degree', 'figurative 2', 1, 2, 'не стал придираться к ней, сделал вид, что ее не заметил')
    (91, '107-APACSadd', 37, 1, 4, 'university degree', 'figurative 2', 2, 2, 'у моего брата никогда нет денег')
    (92, '107-APACSadd', 37, 1, 4, 'university degree', 'figurative 2', 3, 2, 'все время о чем-то думает, о чем-то мечтает, отвлекается')
    (93, '107-APACSadd', 37, 1, 4, 'university degree', 'figurative 2', 4, 2, 'вася просто счастлив')
    (94, '107-APACSadd', 37, 1, 4, 'university degree', 'figurative 2', 5, 2, 'она будет сожалеть')
    (95, '107-APACSadd', 37, 1, 4, 'university degree', 'figurative 2', 6, 2, 'бывают очень тяжелые сумки')
    (96, '107-APACSadd', 37, 1, 4, 'university degree', 'figurative 2', 7, 2, 'некоторые воспоминания очень неприятны')
    (97, '107-APACSadd', 37, 1, 4, 'university degree', 'figurative 2', 8, 1, 'бывают голоса, которые невозможно не услышать; даже когда человек тихо говорит, а его все равно слышно')
    (98, '107-APACSadd', 37, 1, 4, 'university degree', 'figurative 2', 9, 2, 'просто идеальны по всем параметрам')
    (99, '107-APACSadd', 37, 1, 4, 'university degree', 'figurative 2', 10, 2, 'небрежная прическа')
    (100, '107-APACSadd', 37, 1, 4, 'university degree', 'figurative 2', 11, 2, 'любой секрет может быть услышан кем-то другим')
    (101, '107-APACSadd', 37, 1, 4, 'university degree', 'figurative 2', 12, 2, 'человек, который обладает каким-то знанием, но не применяет в своей жизни по каким-то обстоятельствам')
    (102, '107-APACSadd', 37, 1, 4, 'university degree', 'figurative 2', 13, 2, 'большое количество людей не будут ждать одного человека, чтобы начать дело, ради которого они собрались')
    (103, '107-APACSadd', 37, 1, 4, 'university degree', 'figurative 2', 14, 2, 'плохие новости несут еще какие-то плохие новости')
    (104, '107-APACSadd', 37, 1, 4, 'university degree', 'figurative 2', 15, 2, 'когда все хорошо, но что-то все равно портит настроение/качество продукта; есть в чем-то изъян')
    (105, '108-APACSadd', 31, 0, 4, 'university degree', 'figurative 2', 1, 2, 'забил на проблему\n')
    (106, '108-APACSadd', 31, 0, 4, 'university degree', 'figurative 2', 2, 2, 'у него нету денег')
    (107, '108-APACSadd', 31, 0, 4, 'university degree', 'figurative 2', 3, 2, 'ученик в своих мыслях')
    (108, '108-APACSadd', 31, 0, 4, 'university degree', 'figurative 2', 4, 2, 'вася очень счастлив')
    (109, '108-APACSadd', 31, 0, 4, 'university degree', 'figurative 2', 5, 1, 'сделка может провалиться')
    (110, '108-APACSadd', 31, 0, 4, 'university degree', 'figurative 2', 6, 2, 'тяжелые')
    (111, '108-APACSadd', 31, 0, 4, 'university degree', 'figurative 2', 7, 2, 'слишком въедчивые')
    (112, '108-APACSadd', 31, 0, 4, 'university degree', 'figurative 2', 8, 2, 'громкие и звонкие')
    (113, '108-APACSadd', 31, 0, 4, 'university degree', 'figurative 2', 9, 1, 'слишком стройные')
    (114, '108-APACSadd', 31, 0, 4, 'university degree', 'figurative 2', 10, 2, 'многие за собой не следят')
    (115, '108-APACSadd', 31, 0, 4, 'university degree', 'figurative 2', 11, 2, 'правда всегда всплывает, либо кто-то подслушивает')
    (116, '108-APACSadd', 31, 0, 4, 'university degree', 'figurative 2', 12, 0, 'неумелый')
    (117, '108-APACSadd', 31, 0, 4, 'university degree', 'figurative 2', 13, 2, 'когда дело нельзя отлагать')
    (118, '108-APACSadd', 31, 0, 4, 'university degree', 'figurative 2', 14, 2, 'трудности всегда идут друг за другом')
    (119, '108-APACSadd', 31, 0, 4, 'university degree', 'figurative 2', 15, 2, 'даже маленькая песчинка может что-то испортить')
    (120, '109-APACSadd', 34, 0, 4, 'university degree', 'figurative 2', 1, 2, 'не обратил внимание, специально')
    (121, '109-APACSadd', 34, 0, 4, 'university degree', 'figurative 2', 2, 2, 'никогда нет денег')
    (122, '109-APACSadd', 34, 0, 4, 'university degree', 'figurative 2', 3, 2, 'отвлекается, думает о чем-то своем')
    (123, '109-APACSadd', 34, 0, 4, 'university degree', 'figurative 2', 4, 2, 'вася очень рад')
    (124, '109-APACSadd', 34, 0, 4, 'university degree', 'figurative 2', 5, 2, 'марина будет сильно огорчена')
    (125, '109-APACSadd', 34, 0, 4, 'university degree', 'figurative 2', 6, 2, 'бывают очень тяжелые сумки')
    (126, '109-APACSadd', 34, 0, 4, 'university degree', 'figurative 2', 7, 2, 'от некоторых воспоминаний очень тяжело отделаться')
    (127, '109-APACSadd', 34, 0, 4, 'university degree', 'figurative 2', 8, 2, 'очень громкие, звучные голоса')
    (128, '109-APACSadd', 34, 0, 4, 'university degree', 'figurative 2', 9, 2, 'очень красивые, смазливенькие')
    (129, '109-APACSadd', 34, 0, 4, 'university degree', 'figurative 2', 10, 2, 'очень густые, плохо сделанные')
    (130, '109-APACSadd', 34, 0, 4, 'university degree', 'figurative 2', 11, 2, 'никогда нельзя быть уверенным, что даже в пустой комнате тебя не могут подслушать')
    (131, '109-APACSadd', 34, 0, 4, 'university degree', 'figurative 2', 12, 2, 'человек, который что-то умеет делать, но этого у него нет')
    (132, '109-APACSadd', 34, 0, 4, 'university degree', 'figurative 2', 13, 2, 'то и значит, не стоит большой компанией одного человека')
    (133, '109-APACSadd', 34, 0, 4, 'university degree', 'figurative 2', 14, 2, 'одна неприятность ведет за собой череду других неприятностей')
    (134, '109-APACSadd', 34, 0, 4, 'university degree', 'figurative 2', 15, 2, 'даже в чем-то хорошем есть что-то свое плохое')
    (135, '110-APACSadd', 32, 1, 4, 'university degree', 'figurative 2', 1, 1, 'не воспринял это как что-то серьезное, подлежащее наказанию')
    (136, '110-APACSadd', 32, 1, 4, 'university degree', 'figurative 2', 2, 2, 'всегда не хватает денег, транжирит')
    (137, '110-APACSadd', 32, 1, 4, 'university degree', 'figurative 2', 3, 2, 'невнимательный, уходит в фантазии, не сосредоточен')
    (138, '110-APACSadd', 32, 1, 4, 'university degree', 'figurative 2', 4, 2, 'он очень счастлив из-за повышения')
    (139, '110-APACSadd', 32, 1, 4, 'university degree', 'figurative 2', 5, 2, 'будет жалеть об упущенных возможностях')
    (140, '110-APACSadd', 32, 1, 4, 'university degree', 'figurative 2', 6, 2, 'тяжелые')
    (141, '110-APACSadd', 32, 1, 4, 'university degree', 'figurative 2', 7, 2, 'трудно отделаться от этих воспоминаний, вызывают неприятные ощущения')
    (142, '110-APACSadd', 32, 1, 4, 'university degree', 'figurative 2', 8, 2, 'очень громкие')
    (143, '110-APACSadd', 32, 1, 4, 'university degree', 'figurative 2', 9, 2, 'двоякий смысл, красивые, но пустые')
    (144, '110-APACSadd', 32, 1, 4, 'university degree', 'figurative 2', 10, 2, 'очень густые')
    (145, '110-APACSadd', 32, 1, 4, 'university degree', 'figurative 2', 11, 2, 'вас могут услышать, аккуратнее с высказываниями, даже в отсутствие людей вокруг')
    (146, '110-APACSadd', 32, 1, 4, 'university degree', 'figurative 2', 12, 2, 'человек что-то делает, но сам себя этим обеспечить не может')
    (147, '110-APACSadd', 32, 1, 4, 'university degree', 'figurative 2', 13, 2, 'решает большинство')
    (148, '110-APACSadd', 32, 1, 4, 'university degree', 'figurative 2', 14, 2, 'за одним событием возможно последуют остальные такие же')
    (149, '110-APACSadd', 32, 1, 4, 'university degree', 'figurative 2', 15, 2, 'маленькая неприятность, негативный момент в общей положительной ситуации')
    (150, '111-APACSadd', 32, 1, 5, 'phd', 'figurative 2', 1, 2, 'проигнорировал')
    (151, '111-APACSadd', 32, 1, 5, 'phd', 'figurative 2', 2, 2, 'без денег\n')
    (152, '111-APACSadd', 32, 1, 5, 'phd', 'figurative 2', 3, 2, 'рассеян')
    (153, '111-APACSadd', 32, 1, 5, 'phd', 'figurative 2', 4, 2, 'счастлив')
    (154, '111-APACSadd', 32, 1, 5, 'phd', 'figurative 2', 5, 2, 'будет сожалеть')
    (155, '111-APACSadd', 32, 1, 5, 'phd', 'figurative 2', 6, 2, 'тяжелые')
    (156, '111-APACSadd', 32, 1, 5, 'phd', 'figurative 2', 7, 2, 'невозможно избавиться, причиняют боль')
    (157, '111-APACSadd', 32, 1, 5, 'phd', 'figurative 2', 8, 2, 'звонкие, громкие')
    (158, '111-APACSadd', 32, 1, 5, 'phd', 'figurative 2', 9, 2, 'красивые')
    (159, '111-APACSadd', 32, 1, 5, 'phd', 'figurative 2', 10, 2, 'неопрятные')
    (160, '111-APACSadd', 32, 1, 5, 'phd', 'figurative 2', 11, 0, 'никогда не знаешь, где находится проблема')
    (161, '111-APACSadd', 32, 1, 5, 'phd', 'figurative 2', 12, 0, 'человек, работающий по профессии, не владеет своими профессиональными навыками')
    (162, '111-APACSadd', 32, 1, 5, 'phd', 'figurative 2', 13, 1, 'несправедливо, когда за опоздание одного страдает много людей')
    (163, '111-APACSadd', 32, 1, 5, 'phd', 'figurative 2', 14, 2, 'неприятности чаще случаются в количестве больше, чем одна')
    (164, '111-APACSadd', 32, 1, 5, 'phd', 'figurative 2', 15, 2, 'в чем-то хорошем всегда есть что-то плохое')
    (165, '112-APACSadd', 47, 0, 4, 'university degree', 'figurative 2', 1, 2, 'сделал вид, что не заметил')
    (166, '112-APACSadd', 47, 0, 4, 'university degree', 'figurative 2', 2, 2, 'всегда нет денег')
    (167, '112-APACSadd', 47, 0, 4, 'university degree', 'figurative 2', 3, 2, 'мечтатель, невнимательный')
    (168, '112-APACSadd', 47, 0, 4, 'university degree', 'figurative 2', 4, 2, 'счастлив')
    (169, '112-APACSadd', 47, 0, 4, 'university degree', 'figurative 2', 5, 2, 'пожалеет еще')
    (170, '112-APACSadd', 47, 0, 4, 'university degree', 'figurative 2', 6, 2, 'очень тяжелые')
    (171, '112-APACSadd', 47, 0, 4, 'university degree', 'figurative 2', 7, 2, 'вызывают сильные эмоции')
    (172, '112-APACSadd', 47, 0, 4, 'university degree', 'figurative 2', 8, 2, 'очень громкие, зычные')
    (173, '112-APACSadd', 47, 0, 4, 'university degree', 'figurative 2', 9, 1, 'с точеной фигуркой')
    (174, '112-APACSadd', 47, 0, 4, 'university degree', 'figurative 2', 10, 1, 'что-то невообразимое на голове')
    (175, '112-APACSadd', 47, 0, 4, 'university degree', 'figurative 2', 11, 2, 'говорите так, подразумевая, что вас могут услышать')
    (176, '112-APACSadd', 47, 0, 4, 'university degree', 'figurative 2', 12, 0, 'работает по специальности, но не может сделать то, чему обучен заниматься')
    (177, '112-APACSadd', 47, 0, 4, 'university degree', 'figurative 2', 13, 2, 'компания не ждет одного опоздавшего')
    (178, '112-APACSadd', 47, 0, 4, 'university degree', 'figurative 2', 14, 2, 'если начались проблемы, то они косяком идут')
    (179, '112-APACSadd', 47, 0, 4, 'university degree', 'figurative 2', 15, 2, 'любое хорошее дело можно испортить каким-либо маленьким, непродуктивным действием')
    (180, '113-APACSadd', 54, 1, 2, 'secondary vocational', 'figurative 2', 1, 2, 'не обратил внимания')
    (181, '113-APACSadd', 54, 1, 2, 'secondary vocational', 'figurative 2', 2, 2, 'нет денег никогда')
    (182, '113-APACSadd', 54, 1, 2, 'secondary vocational', 'figurative 2', 3, 2, 'не слушает, занят сам собой')
    (183, '113-APACSadd', 54, 1, 2, 'secondary vocational', 'figurative 2', 4, 2, 'счастлив')
    (184, '113-APACSadd', 54, 1, 2, 'secondary vocational', 'figurative 2', 5, 2, 'расстроится')
    (185, '113-APACSadd', 54, 1, 2, 'secondary vocational', 'figurative 2', 6, 2, 'очень тяжелые сумки')
    (186, '113-APACSadd', 54, 1, 2, 'secondary vocational', 'figurative 2', 7, 2, 'плохие, ужасные')
    (187, '113-APACSadd', 54, 1, 2, 'secondary vocational', 'figurative 2', 8, 2, 'громкие, звонкие')
    (188, '113-APACSadd', 54, 1, 2, 'secondary vocational', 'figurative 2', 9, 2, 'очень красивые')
    (189, '113-APACSadd', 54, 1, 2, 'secondary vocational', 'figurative 2', 10, 2, 'неопрятные, некрасивые')
    (190, '113-APACSadd', 54, 1, 2, 'secondary vocational', 'figurative 2', 11, 2, 'подслушивают все везде, могут узнать свои секреты')
    (191, '113-APACSadd', 54, 1, 2, 'secondary vocational', 'figurative 2', 12, 2, 'человек в профессии и не имеет того, чем занимается')
    (192, '113-APACSadd', 54, 1, 2, 'secondary vocational', 'figurative 2', 13, 1, 'если ты не успел, тебя никто не ждет, допустим, группа, один остался, не доделал, тебя никто ждать не будет')
    (193, '113-APACSadd', 54, 1, 2, 'secondary vocational', 'figurative 2', 14, 1, 'бывает такое, все навалилось')
    (194, '113-APACSadd', 54, 1, 2, 'secondary vocational', 'figurative 2', 15, 2, 'все хорошо, но какая-то маленькая мелочь все портит')
    (195, '114-APACSadd', 18, 0, 1, 'incomplete secondary vocational', 'figurative 2', 1, 2, 'ей все равно')
    (196, '114-APACSadd', 18, 0, 1, 'incomplete secondary vocational', 'figurative 2', 2, 0, 'не знает')
    (197, '114-APACSadd', 18, 0, 1, 'incomplete secondary vocational', 'figurative 2', 3, 2, 'не сконцентрирован')
    (198, '114-APACSadd', 18, 0, 1, 'incomplete secondary vocational', 'figurative 2', 4, 2, 'счастлив')
    (199, '114-APACSadd', 18, 0, 1, 'incomplete secondary vocational', 'figurative 2', 5, 2, 'переживать')
    (200, '114-APACSadd', 18, 0, 1, 'incomplete secondary vocational', 'figurative 2', 6, 2, 'тяжелые')
    (201, '114-APACSadd', 18, 0, 1, 'incomplete secondary vocational', 'figurative 2', 7, 2, 'незабываемые, неприятные')
    (202, '114-APACSadd', 18, 0, 1, 'incomplete secondary vocational', 'figurative 2', 8, 0, 'не понимает, что значит')
    (203, '114-APACSadd', 18, 0, 1, 'incomplete secondary vocational', 'figurative 2', 9, 2, 'красивые')
    (204, '114-APACSadd', 18, 0, 1, 'incomplete secondary vocational', 'figurative 2', 10, 2, 'лохматые\n')
    (205, '114-APACSadd', 18, 0, 1, 'incomplete secondary vocational', 'figurative 2', 11, 1, 'нам так тренер говорит, чтобы мы ничего не уносили')
    (206, '114-APACSadd', 18, 0, 1, 'incomplete secondary vocational', 'figurative 2', 12, 0, 'бедный, наверное')
    (207, '114-APACSadd', 18, 0, 1, 'incomplete secondary vocational', 'figurative 2', 13, 2, 'не ждут много человек одного')
    (208, '114-APACSadd', 18, 0, 1, 'incomplete secondary vocational', 'figurative 2', 14, 2, 'за бедой будет еще несколько проблем')
    (209, '114-APACSadd', 18, 0, 1, 'incomplete secondary vocational', 'figurative 2', 15, 0, 'не знаю')
    (210, '115-APACSadd', 32, 1, 5, 'phd', 'figurative 2', 1, 2, 'решил не обращать внимание')
    (211, '115-APACSadd', 32, 1, 5, 'phd', 'figurative 2', 2, 2, 'у него всегда нет денег, или недостаточно денег')
    (212, '115-APACSadd', 32, 1, 5, 'phd', 'figurative 2', 3, 1, 'не очень усердный, не занимается')
    (213, '115-APACSadd', 32, 1, 5, 'phd', 'figurative 2', 4, 2, 'счастлив очень')
    (214, '115-APACSadd', 32, 1, 5, 'phd', 'figurative 2', 5, 2, 'досадовать о чем-то, каком-то прошедшем событии')
    (215, '115-APACSadd', 32, 1, 5, 'phd', 'figurative 2', 6, 2, 'очень тяжелые')
    (216, '115-APACSadd', 32, 1, 5, 'phd', 'figurative 2', 7, 2, 'постоянно досаждающие, причиняющие боль')
    (217, '115-APACSadd', 32, 1, 5, 'phd', 'figurative 2', 8, 2, 'очень громкие, видимо')
    (218, '115-APACSadd', 32, 1, 5, 'phd', 'figurative 2', 9, 2, 'красавицы')
    (219, '115-APACSadd', 32, 1, 5, 'phd', 'figurative 2', 10, 2, 'непричесанные, взъерошенные, шевелюры')
    (220, '115-APACSadd', 32, 1, 5, 'phd', 'figurative 2', 11, 2, 'всегда кто-то может подслушать, что говоришь; все становится явным')
    (221, '115-APACSadd', 32, 1, 5, 'phd', 'figurative 2', 12, 0, 'не специалист своего дела, даже в своем деле не специалист')
    (222, '115-APACSadd', 32, 1, 5, 'phd', 'figurative 2', 13, 2, 'не следует ждать одного человека всей компанией')
    (223, '115-APACSadd', 32, 1, 5, 'phd', 'figurative 2', 14, 2, 'черная полоса; тревожные события следуют по цепочке одно за другим')
    (224, '115-APACSadd', 32, 1, 5, 'phd', 'figurative 2', 15, 2, 'неприятный нюанс в чем-то хорошем')
    (225, '116-APACSadd', 28, 0, 5, 'phd', 'figurative 2', 1, 2, 'не обратил внимание')
    (226, '116-APACSadd', 28, 0, 5, 'phd', 'figurative 2', 2, 2, 'испытывает финансовые трудности')
    (227, '116-APACSadd', 28, 0, 5, 'phd', 'figurative 2', 3, 2, 'теряет концентрацию внимания')
    (228, '116-APACSadd', 28, 0, 5, 'phd', 'figurative 2', 4, 2, 'вася испытывает сильные положительные эмоции, из-за которых он перестает обращать внимание на окружающую действительность')
    (229, '116-APACSadd', 28, 0, 5, 'phd', 'figurative 2', 5, 0, 'завидовать')
    (230, '116-APACSadd', 28, 0, 5, 'phd', 'figurative 2', 6, 2, 'очень тяжелые сумки\n')
    (231, '116-APACSadd', 28, 0, 5, 'phd', 'figurative 2', 7, 2, 'не стираются из памяти и доставляют неприятные эмоции')
    (232, '116-APACSadd', 28, 0, 5, 'phd', 'figurative 2', 8, 2, 'очень звонкие голоса')
    (233, '116-APACSadd', 28, 0, 5, 'phd', 'figurative 2', 9, 1, 'просто очень разукрашенные')
    (234, '116-APACSadd', 28, 0, 5, 'phd', 'figurative 2', 10, 2, 'густые прически')
    (235, '116-APACSadd', 28, 0, 5, 'phd', 'figurative 2', 11, 0, 'кто-то может слушать через стены\n')
    (236, '116-APACSadd', 28, 0, 5, 'phd', 'figurative 2', 12, 1, 'бедный, о всех заботится кроме себя, сам чинит всем сапоги, а у самого сапог нет\n')
    (237, '116-APACSadd', 28, 0, 5, 'phd', 'figurative 2', 13, 2, 'большинство не должно ждать меньшинство\n')
    (238, '116-APACSadd', 28, 0, 5, 'phd', 'figurative 2', 14, 2, 'закон парных случаев; случается несчастье, и за ним следует еще одно несчастье')
    (239, '116-APACSadd', 28, 0, 5, 'phd', 'figurative 2', 15, 2, 'вроде бы все хорошо, но есть неприятный осадок')
    (240, '117-APACSadd', 27, 1, 5, 'phd', 'figurative 2', 1, 2, 'не стал обращать внимание на поведение, не обратил внимание')
    (241, '117-APACSadd', 27, 1, 5, 'phd', 'figurative 2', 2, 2, 'у моего брата нет денег')
    (242, '117-APACSadd', 27, 1, 5, 'phd', 'figurative 2', 3, 2, 'рассеянный')
    (243, '117-APACSadd', 27, 1, 5, 'phd', 'figurative 2', 4, 2, 'вася очень рад')
    (244, '117-APACSadd', 27, 1, 5, 'phd', 'figurative 2', 5, 0, 'будет чувствовать угрызения совести')
    (245, '117-APACSadd', 27, 1, 5, 'phd', 'figurative 2', 6, 2, 'очень тяжелые\n')
    (246, '117-APACSadd', 27, 1, 5, 'phd', 'figurative 2', 7, 2, 'у человека есть неприятные воспоминания')
    (247, '117-APACSadd', 27, 1, 5, 'phd', 'figurative 2', 8, 2, 'звонкие голоса')
    (248, '117-APACSadd', 27, 1, 5, 'phd', 'figurative 2', 9, 2, 'очень красивые')
    (249, '117-APACSadd', 27, 1, 5, 'phd', 'figurative 2', 10, 2, 'неаккуратные прически, нестриженные')
    (250, '117-APACSadd', 27, 1, 5, 'phd', 'figurative 2', 11, 2, 'не стоит раскрывать секреты, все подслушают, все узнают')
    (251, '117-APACSadd', 27, 1, 5, 'phd', 'figurative 2', 12, 0, 'когда есть изъяны в своей работе, например, учитель английского языка, допустим, на детей не хватает времени')
    (252, '117-APACSadd', 27, 1, 5, 'phd', 'figurative 2', 13, 2, 'чтобы не опаздывали; не опаздывать')
    (253, '117-APACSadd', 27, 1, 5, 'phd', 'figurative 2', 14, 1, 'всегда за чем-то не приходит что-то плохое однократно, обязательно что-то происходит в совокупности')
    (254, '117-APACSadd', 27, 1, 5, 'phd', 'figurative 2', 15, 1, 'есть обязательно один человек который против')
    (255, '118-APACSadd', 38, 0, 4, 'university degree', 'figurative 2', 1, 2, 'не обратил внимание специально')
    (256, '118-APACSadd', 38, 0, 4, 'university degree', 'figurative 2', 2, 2, 'у брата всегда нет денег')
    (257, '118-APACSadd', 38, 0, 4, 'university degree', 'figurative 2', 3, 2, 'постоянно мечтает')
    (258, '118-APACSadd', 38, 0, 4, 'university degree', 'figurative 2', 4, 2, 'очень рад повышению')
    (259, '118-APACSadd', 38, 0, 4, 'university degree', 'figurative 2', 5, 2, 'будет переживать из-за нее, расстраиваться')
    (260, '118-APACSadd', 38, 0, 4, 'university degree', 'figurative 2', 6, 2, 'очень тяжелые сумки бывают')
    (261, '118-APACSadd', 38, 0, 4, 'university degree', 'figurative 2', 7, 2, 'неприятные, и сложно избавиться от них')
    (262, '118-APACSadd', 38, 0, 4, 'university degree', 'figurative 2', 8, 2, 'очень громкие, зычные')
    (263, '118-APACSadd', 38, 0, 4, 'university degree', 'figurative 2', 9, 2, 'очень тонкие, стройные; а может, и красивые')
    (264, '118-APACSadd', 38, 0, 4, 'university degree', 'figurative 2', 10, 2, 'спутанные')
    (265, '118-APACSadd', 38, 0, 4, 'university degree', 'figurative 2', 11, 2, 'кто-то может подслушать всегда')
    (266, '118-APACSadd', 38, 0, 4, 'university degree', 'figurative 2', 12, 2, 'сам чем-то человек занимается, а самого этого у человека нет')
    (267, '118-APACSadd', 38, 0, 4, 'university degree', 'figurative 2', 13, 2, 'не надо опаздывать, когда собирается много людей')
    (268, '118-APACSadd', 38, 0, 4, 'university degree', 'figurative 2', 14, 2, 'если что-то плохое случится, скорее всего потом случится еще что-то плохое')
    (269, '118-APACSadd', 38, 0, 4, 'university degree', 'figurative 2', 15, 2, 'одна незначительная неприятная деталь может испортить общее положительное впечатление о чем-то\n')
    (270, '119-APACSadd', 21, 0, 3, 'incomplete university degree', 'figurative 2', 1, 1, 'отпустил ее, пропустил мимо ушей, забил')
    (271, '119-APACSadd', 21, 0, 3, 'incomplete university degree', 'figurative 2', 2, 2, 'у него никогда нет денег')
    (272, '119-APACSadd', 21, 0, 3, 'incomplete university degree', 'figurative 2', 3, 1, 'фантазер')
    (273, '119-APACSadd', 21, 0, 3, 'incomplete university degree', 'figurative 2', 4, 2, 'вася очень счастлив')
    (274, '119-APACSadd', 21, 0, 3, 'incomplete university degree', 'figurative 2', 5, 1, 'еще поплатится за это')
    (275, '119-APACSadd', 21, 0, 3, 'incomplete university degree', 'figurative 2', 6, 2, 'очень тяжелые сумки')
    (276, '119-APACSadd', 21, 0, 3, 'incomplete university degree', 'figurative 2', 7, 2, 'острые воспоминания, имеют эмоциональный отклик в голове, засели')
    (277, '119-APACSadd', 21, 0, 3, 'incomplete university degree', 'figurative 2', 8, 2, 'очень громкие голоса\n')
    (278, '119-APACSadd', 21, 0, 3, 'incomplete university degree', 'figurative 2', 9, 2, 'очень красивые')
    (279, '119-APACSadd', 21, 0, 3, 'incomplete university degree', 'figurative 2', 10, 2, 'очень длинные волосы, неряшливые')
    (280, '119-APACSadd', 21, 0, 3, 'incomplete university degree', 'figurative 2', 11, 2, 'подслушать может каждый, из самого неожиданного места')
    (281, '119-APACSadd', 21, 0, 3, 'incomplete university degree', 'figurative 2', 12, 0, 'человек, который много ругается')
    (282, '119-APACSadd', 21, 0, 3, 'incomplete university degree', 'figurative 2', 13, 2, 'большинство всегда уйдет без одного человека, так как не имеет смысла ждать; неуважительно по отношению к большинству')
    (283, '119-APACSadd', 21, 0, 3, 'incomplete university degree', 'figurative 2', 14, 2, 'от одной беды может произойти еще больше')
    (284, '119-APACSadd', 21, 0, 3, 'incomplete university degree', 'figurative 2', 15, 2, 'какое-то маленькое событие, которое испортило все, что было хорошее')
    (285, '120-APACSadd', 33, 0, 4, 'university degree', 'figurative 2', 1, 0, 'устал уже от особ такого поведения, устал')
    (286, '120-APACSadd', 33, 0, 4, 'university degree', 'figurative 2', 2, 2, 'у него нет никогда денег')
    (287, '120-APACSadd', 33, 0, 4, 'university degree', 'figurative 2', 3, 2, 'мечтатель')
    (288, '120-APACSadd', 33, 0, 4, 'university degree', 'figurative 2', 4, 2, 'восторг, радость')
    (289, '120-APACSadd', 33, 0, 4, 'university degree', 'figurative 2', 5, 2, 'обидно стало марине, станет')
    (290, '120-APACSadd', 33, 0, 4, 'university degree', 'figurative 2', 6, 2, 'очень тяжелые сумки')
    (291, '120-APACSadd', 33, 0, 4, 'university degree', 'figurative 2', 7, 2, 'больные, плохие; болезненные')
    (292, '120-APACSadd', 33, 0, 4, 'university degree', 'figurative 2', 8, 2, 'ну очень сильный голос')
    (293, '120-APACSadd', 33, 0, 4, 'university degree', 'figurative 2', 9, 2, 'выглядят очень красиво')
    (294, '120-APACSadd', 33, 0, 4, 'university degree', 'figurative 2', 10, 1, 'оригинальные прически')
    (295, '120-APACSadd', 33, 0, 4, 'university degree', 'figurative 2', 11, 2, 'ничего не скроешь')
    (296, '120-APACSadd', 33, 0, 4, 'university degree', 'figurative 2', 12, 1, 'несчастный, есть у него несчастье в этом\n')
    (297, '120-APACSadd', 33, 0, 4, 'university degree', 'figurative 2', 13, 1, 'ненавижу опоздания')
    (298, '120-APACSadd', 33, 0, 4, 'university degree', 'figurative 2', 14, 2, 'если начинаются проблемы, то идут прям скопом')
    (299, '120-APACSadd', 33, 0, 4, 'university degree', 'figurative 2', 15, 2, 'человеческое несовершенство; даже если все хорошо, что-нибудь да будет плохо, на сто процентов не будет идеал')
    (300, '121-APACSadd', 72, 1, 2, 'secondary vocational', 'figurative 2', 1, 2, 'не обратил внимание')
    (301, '121-APACSadd', 72, 1, 2, 'secondary vocational', 'figurative 2', 2, 2, 'без денег')
    (302, '121-APACSadd', 72, 1, 2, 'secondary vocational', 'figurative 2', 3, 2, 'сидит мечтает, ничего не делает, мечтатель')
    (303, '121-APACSadd', 72, 1, 2, 'secondary vocational', 'figurative 2', 4, 2, 'от радости что повысили')
    (304, '121-APACSadd', 72, 1, 2, 'secondary vocational', 'figurative 2', 5, 2, 'будет жалеть')
    (305, '121-APACSadd', 72, 1, 2, 'secondary vocational', 'figurative 2', 6, 2, 'тяжелые')
    (306, '121-APACSadd', 72, 1, 2, 'secondary vocational', 'figurative 2', 7, 2, 'тяжелые воспоминания')
    (307, '121-APACSadd', 72, 1, 2, 'secondary vocational', 'figurative 2', 8, 0, 'простуженные')
    (308, '121-APACSadd', 72, 1, 2, 'secondary vocational', 'figurative 2', 9, 1, 'хорошо одеты')
    (309, '121-APACSadd', 72, 1, 2, 'secondary vocational', 'figurative 2', 10, 2, 'густые волосы')
    (310, '121-APACSadd', 72, 1, 2, 'secondary vocational', 'figurative 2', 11, 2, 'когда подслушивают')
    (311, '121-APACSadd', 72, 1, 2, 'secondary vocational', 'figurative 2', 12, 1, 'например, шьет обувь, а сам ходит без обуви')
    (312, '121-APACSadd', 72, 1, 2, 'secondary vocational', 'figurative 2', 13, 2, 'опаздывает если, пусть идет своим путем, а они идут куда надо')
    (313, '121-APACSadd', 72, 1, 2, 'secondary vocational', 'figurative 2', 14, 2, 'одна беда случилась, и другая может случиться, или заблеют или еще что')
    (314, '121-APACSadd', 72, 1, 2, 'secondary vocational', 'figurative 2', 15, 2, 'например, что-то хорошее, и обязательно найдется немножечко плохого')
    (315, '122-APACSadd', 65, 1, 4, 'university degree', 'figurative 2', 1, 2, 'проигнорировал\n')
    (316, '122-APACSadd', 65, 1, 4, 'university degree', 'figurative 2', 2, 2, 'вечно ничего нету, все истратил')
    (317, '122-APACSadd', 65, 1, 4, 'university degree', 'figurative 2', 3, 2, 'думает о своем')
    (318, '122-APACSadd', 65, 1, 4, 'university degree', 'figurative 2', 4, 2, 'рад, счастлив')
    (319, '122-APACSadd', 65, 1, 4, 'university degree', 'figurative 2', 5, 2, 'разочарование какое-то')
    (320, '122-APACSadd', 65, 1, 4, 'university degree', 'figurative 2', 6, 2, 'тяжелые')
    (321, '122-APACSadd', 65, 1, 4, 'university degree', 'figurative 2', 7, 0, 'в душу запали, добрые')
    (322, '122-APACSadd', 65, 1, 4, 'university degree', 'figurative 2', 8, 1, 'грубые')
    (323, '122-APACSadd', 65, 1, 4, 'university degree', 'figurative 2', 9, 2, 'красивые')
    (324, '122-APACSadd', 65, 1, 4, 'university degree', 'figurative 2', 10, 2, 'лохматые')
    (325, '122-APACSadd', 65, 1, 4, 'university degree', 'figurative 2', 11, 2, 'осторожно надо, чтобы тайну не выдал')
    (326, '122-APACSadd', 65, 1, 4, 'university degree', 'figurative 2', 12, 0, 'лодырь, лентяй, расхлябанный')
    (327, '122-APACSadd', 65, 1, 4, 'university degree', 'figurative 2', 13, 2, 'надо быстро собираться')
    (328, '122-APACSadd', 65, 1, 4, 'university degree', 'figurative 2', 14, 2, 'не везет челвеку, постоянные какие-то неурядицы, невезуха')
    (329, '122-APACSadd', 65, 1, 4, 'university degree', 'figurative 2', 15, 2, 'одна неприятность портит жизнь')
    (330, '123-APACSadd', 27, 1, 5, 'phd', 'figurative 2', 1, 2, 'проигнорировал ее поведение')
    (331, '123-APACSadd', 27, 1, 5, 'phd', 'figurative 2', 2, 2, 'у моего брата всегда нет денег')
    (332, '123-APACSadd', 27, 1, 5, 'phd', 'figurative 2', 3, 2, 'этот ученик постоянно погружен в свои мысли')
    (333, '123-APACSadd', 27, 1, 5, 'phd', 'figurative 2', 4, 2, 'вася очень рад своему повышению')
    (334, '123-APACSadd', 27, 1, 5, 'phd', 'figurative 2', 5, 2, 'из-за этой сделки марина потом расстроится')
    (335, '123-APACSadd', 27, 1, 5, 'phd', 'figurative 2', 6, 2, 'бывают очень тяжелые сумки')
    (336, '123-APACSadd', 27, 1, 5, 'phd', 'figurative 2', 7, 2, 'некоторые воспоминания очень болезненные')
    (337, '123-APACSadd', 27, 1, 5, 'phd', 'figurative 2', 8, 2, 'бывают очень громкие голоса')
    (338, '123-APACSadd', 27, 1, 5, 'phd', 'figurative 2', 9, 2, 'есть очень красивые манекенщицы')
    (339, '123-APACSadd', 27, 1, 5, 'phd', 'figurative 2', 10, 2, 'прически у некоторых очень неряшливые')
    (340, '123-APACSadd', 27, 1, 5, 'phd', 'figurative 2', 11, 2, 'любую информацию можно подслушать')
    (341, '123-APACSadd', 27, 1, 5, 'phd', 'figurative 2', 12, 1, 'когда человек должен свою профессию хорошо выполнять, но при этом с ней не справляется')
    (342, '123-APACSadd', 27, 1, 5, 'phd', 'figurative 2', 13, 2, 'люди не должны терять время из-за невнимательности одного человека')
    (343, '123-APACSadd', 27, 1, 5, 'phd', 'figurative 2', 14, 2, 'если начались неприятности, их всегда происходит много, черная полоса\n')
    (344, '123-APACSadd', 27, 1, 5, 'phd', 'figurative 2', 15, 2, 'что-то, что портит всю картину')
    (345, '124-APACSadd', 73, 1, 4, 'university degree', 'figurative 2', 1, 2, 'не обратил внимание на ее поведение')
    (346, '124-APACSadd', 73, 1, 4, 'university degree', 'figurative 2', 2, 2, 'у моего брата никогда не бывает денег')
    (347, '124-APACSadd', 73, 1, 4, 'university degree', 'figurative 2', 3, 2, 'этот ученик постоянно в мечтах')
    (348, '124-APACSadd', 73, 1, 4, 'university degree', 'figurative 2', 4, 2, 'вася очень рад повышению')
    (349, '124-APACSadd', 73, 1, 4, 'university degree', 'figurative 2', 5, 2, 'марина еще пожалеет')
    (350, '124-APACSadd', 73, 1, 4, 'university degree', 'figurative 2', 6, 2, 'бывают очень тяжелые сумки')
    (351, '124-APACSadd', 73, 1, 4, 'university degree', 'figurative 2', 7, 2, 'некоторые воспоминания очень болезненны')
    (352, '124-APACSadd', 73, 1, 4, 'university degree', 'figurative 2', 8, 2, 'бывают очень громкие голоса')
    (353, '124-APACSadd', 73, 1, 4, 'university degree', 'figurative 2', 9, 2, 'есть очень красивые манекенщицы')
    (354, '124-APACSadd', 73, 1, 4, 'university degree', 'figurative 2', 10, 0, 'взлохмаченные? не знаю')
    (355, '124-APACSadd', 73, 1, 4, 'university degree', 'figurative 2', 11, 2, 'вас могут всегда услышать')
    (356, '124-APACSadd', 73, 1, 4, 'university degree', 'figurative 2', 12, 2, 'у любого человека иногда не бывает того, что он производит')
    (357, '124-APACSadd', 73, 1, 4, 'university degree', 'figurative 2', 13, 2, 'что компания одного опоздавшего не ждет')
    (358, '124-APACSadd', 73, 1, 4, 'university degree', 'figurative 2', 14, 2, 'если на тебя свалилась одна беда, жди другую')
    (359, '124-APACSadd', 73, 1, 4, 'university degree', 'figurative 2', 15, 2, 'когда есть что-то хорошее, обязательно бывает хоть маленькая, но гадость')
    


```python
con.commit()
```

Step 2. Queries

Now that we have this database, let's make six different queries to it.


```python
# Querie 1. Let's see the best answers that the participant 103-APACSadd made during this task.

for i in con.execute("SELECT * FROM pdata WHERE Id='103-APACSadd' AND Score='2'"):
    print(i)
```

    (30, '103-APACSadd', 21, 0, 4, 'university degree', 'figurative 2', 1, 2, 'не стал обращать внимание на то, как она себя ведет')
    (31, '103-APACSadd', 21, 0, 4, 'university degree', 'figurative 2', 2, 2, 'у него нет денег')
    (32, '103-APACSadd', 21, 0, 4, 'university degree', 'figurative 2', 3, 2, 'отвлекается все время')
    (33, '103-APACSadd', 21, 0, 4, 'university degree', 'figurative 2', 4, 2, 'очень счастлив')
    (34, '103-APACSadd', 21, 0, 4, 'university degree', 'figurative 2', 5, 2, 'будет недовольна')
    (35, '103-APACSadd', 21, 0, 4, 'university degree', 'figurative 2', 6, 2, 'тяжелые сумки')
    (36, '103-APACSadd', 21, 0, 4, 'university degree', 'figurative 2', 7, 2, 'их сложно забыть')
    (37, '103-APACSadd', 21, 0, 4, 'university degree', 'figurative 2', 8, 2, 'очень громкие голоса')
    (38, '103-APACSadd', 21, 0, 4, 'university degree', 'figurative 2', 9, 2, 'очень красивые манекенщицы')
    (39, '103-APACSadd', 21, 0, 4, 'university degree', 'figurative 2', 10, 2, 'очень густые прически')
    (40, '103-APACSadd', 21, 0, 4, 'university degree', 'figurative 2', 11, 2, 'надо аккуратно разговаривать')
    (42, '103-APACSadd', 21, 0, 4, 'university degree', 'figurative 2', 13, 2, 'мнение большинства важнее мнения одного')
    (43, '103-APACSadd', 21, 0, 4, 'university degree', 'figurative 2', 14, 2, 'если что-то плохо, то что-то тоже будет плохо')
    (44, '103-APACSadd', 21, 0, 4, 'university degree', 'figurative 2', 15, 2, 'плохая мелочь, которая портит всю картину')
    


```python
# Querie 2. Let's organize our participants' scores for the 12th utterance from the highest score to the lowest.
# Also, let's restrict the output to their ID's and ages.
# (Oh, by the way, the utterance in question here is "Сапожник без сапог").

for i in con.execute("SELECT Id, Age, Item, Score, Answer FROM pdata WHERE Item='12' ORDER BY Score DESC"):
    print(i)
```

    ('101-APACSadd', 21, 12, 2, 'обслуживает всех кроме себя')
    ('102-APACSadd', 37, 12, 2, 'человек, который занимается конкретным делом, не может применить это дело к себе')
    ('104-APACSadd', 50, 12, 2, 'чужим что-то делает, а у самого нету')
    ('105-APACSadd', 53, 12, 2, 'профессионал часто оказывается не в состоянии помочь самому себе')
    ('107-APACSadd', 37, 12, 2, 'человек, который обладает каким-то знанием, но не применяет в своей жизни по каким-то обстоятельствам')
    ('109-APACSadd', 34, 12, 2, 'человек, который что-то умеет делать, но этого у него нет')
    ('110-APACSadd', 32, 12, 2, 'человек что-то делает, но сам себя этим обеспечить не может')
    ('113-APACSadd', 54, 12, 2, 'человек в профессии и не имеет того, чем занимается')
    ('118-APACSadd', 38, 12, 2, 'сам чем-то человек занимается, а самого этого у человека нет')
    ('124-APACSadd', 73, 12, 2, 'у любого человека иногда не бывает того, что он производит')
    ('116-APACSadd', 28, 12, 1, 'бедный, о всех заботится кроме себя, сам чинит всем сапоги, а у самого сапог нет\n')
    ('120-APACSadd', 33, 12, 1, 'несчастный, есть у него несчастье в этом\n')
    ('121-APACSadd', 72, 12, 1, 'например, шьет обувь, а сам ходит без обуви')
    ('123-APACSadd', 27, 12, 1, 'когда человек должен свою профессию хорошо выполнять, но при этом с ней не справляется')
    ('103-APACSadd', 21, 12, 0, 'человек, который нарушает ожидание; любой человек ожидает чего-то одного, а он этого не делает или не умеет; есть какое-то разочарование')
    ('106-APACSadd', 42, 12, 0, 'не умеет делать что-то, как правило, свою работу')
    ('108-APACSadd', 31, 12, 0, 'неумелый')
    ('111-APACSadd', 32, 12, 0, 'человек, работающий по профессии, не владеет своими профессиональными навыками')
    ('112-APACSadd', 47, 12, 0, 'работает по специальности, но не может сделать то, чему обучен заниматься')
    ('114-APACSadd', 18, 12, 0, 'бедный, наверное')
    ('115-APACSadd', 32, 12, 0, 'не специалист своего дела, даже в своем деле не специалист')
    ('117-APACSadd', 27, 12, 0, 'когда есть изъяны в своей работе, например, учитель английского языка, допустим, на детей не хватает времени')
    ('119-APACSadd', 21, 12, 0, 'человек, который много ругается')
    ('122-APACSadd', 65, 12, 0, 'лодырь, лентяй, расхлябанный')
    


```python
# Querie 3. Let's see what are 106-APACSadd's last 5 answers and scores.

for i in con.execute("SELECT Id, Item, Score, Answer FROM pdata WHERE Id = '106-APACSadd' LIMIT 5 OFFSET 10"):
    print(i)
```

    ('106-APACSadd', 11, 2, 'слышат все, подслушивают')
    ('106-APACSadd', 12, 0, 'не умеет делать что-то, как правило, свою работу')
    ('106-APACSadd', 13, 2, 'много людей не ждет одного')
    ('106-APACSadd', 14, 2, 'если что-то случилось, как правило будет несколько неприятностей')
    ('106-APACSadd', 15, 2, 'все хорошо, но что-то малое портит впечатление')
    


```python
# Querie 4. Let's see what 102-APACSadd's total score is.

for i in con.execute('SELECT Id, SUM(Score) AS Total_score FROM pdata WHERE Id="102-APACSadd"'):
    print(i)
```

    ('102-APACSadd', 29)
    


```python
# Querie 5. Let's find the average score for each item.

for i in con.execute('SELECT Item, AVG(Score) AS Total_score FROM pdata GROUP BY Item'):
    print(i)
```

    (1, 1.75)
    (2, 1.9166666666666667)
    (3, 1.875)
    (4, 2.0)
    (5, 1.75)
    (6, 2.0)
    (7, 1.9166666666666667)
    (8, 1.6666666666666667)
    (9, 1.8333333333333333)
    (10, 1.7916666666666667)
    (11, 1.7916666666666667)
    (12, 1.0)
    (13, 1.875)
    (14, 1.9166666666666667)
    (15, 1.8333333333333333)
    


```python
# Querie 6. (I think I'm running out of ideas).
# Let's order these averages from the lowest score to the highest, 
# to see which item proved to be the most difficult to comprehend and explain.

for i in con.execute('SELECT Item, AVG(Score) AS Total_score FROM pdata GROUP BY Item ORDER BY AVG(Score) ASC'):
    print(i)
```

    (12, 1.0)
    (8, 1.6666666666666667)
    (1, 1.75)
    (5, 1.75)
    (10, 1.7916666666666667)
    (11, 1.7916666666666667)
    (9, 1.8333333333333333)
    (15, 1.8333333333333333)
    (3, 1.875)
    (13, 1.875)
    (2, 1.9166666666666667)
    (7, 1.9166666666666667)
    (14, 1.9166666666666667)
    (4, 2.0)
    (6, 2.0)
    

Step 3. Visualization

Here I must admit that I have created only three plots out of four, two of them being barplots (so in all honesty what I did is made two kinds of plots instead of four), either because there is not much to be visualized in my data, especially in the 'db2' table, or I just lack imagination. 


```python
# importing libraries
import matplotlib.pyplot as plt
%matplotlib inline
```


```python
# Plot 1. Let's see the gender ratio of our participants.
# For this barplot I'll be using the first table, because there are no duplicate rows.

df1['Gender'].value_counts().plot.bar(color=['orange','purple']); # yay halloween!
plt.title('Gender ratio')
plt.xlabel('gender (1 = female, 0 = male)')
plt.ylabel('number of participants');
```


![png](output_23_0.png)



```python
# Plot 2. Let's visualize the ages of the participants.
# Ideally, I wanted it to show me the ages in an ascending order,
# but I didn't figure out how to do it, and, besides,
# maybe it's not that necessary in this case.

df1['Age'].plot(kind="bar", color="teal");
plt.title('Age of the participants')
plt.xlabel('participant')
plt.ylabel('age')
```




    Text(0, 0.5, 'age')




![png](output_24_1.png)



```python
# Plot 3. Let's create a pie chart. It will show us the level of education ratio -- or, in other words,
# how many participants belong to this or that education group.

plt.figure(figsize=(6, 6))
df1['EducGroupCat'].value_counts().plot(kind='pie');
plt.title('Education group categories');
```


![png](output_25_0.png)



```python
con.close()
```

Step 4. Final project

Unfortunately, I haven't really figured out what I want to do for my final project. Is it possible to send you an e-mail sometime later (maybe in a week or so)?
