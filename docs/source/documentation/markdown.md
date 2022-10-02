# Markdown Tips often slipped your mind

## 1. Basic from the basic
대표적인 웹 markup인 HTML은 문서의 내용에 중점을 두자는 철학으로 정립되었다. 꾸밈이나 작동은 각각 css와 javascript 등에게 담당케하여 본질적인 내용만을 담당하는 형태로 정착되었다.   

이런 HTML 도 마컵 과정이 좀 심하게 번잡하다. 유명 에디터의 도움없이 작성하기란 타이핑 지옥을 체험하는 일이란 생각이 들 정도이다. 구성 철학과는 다르게 내용에 집중해서 타이핑하기가 쉽지 않은 언어라는 모순에 빠진다.

이런 문제를 해결해 준 것이 바로 HTML로 변환이 간편하게 제작된  markdown이란 마컵언어이다. 웹 환경에서 현재로서는 최적화되고 단순, 간편한 타이핑 룰을 가진 언어라고 느끼기에 부족함이 없다.

그럼에도 불구하고 공백처리와 링크, 리스트 정돈 등에서 종종 까먹는 syntax들이 꾀 된다.

## 2. `&`와 긴 공백 삽입
- escape characters로 다음과 같은 특별한 것들이 있다. `&` : ampersands  
    |기호|이름|HTML  |  Markdown | 예제결과   |
    |:---:|:---|---|---|---|
    |`&`  |ampersands |`AT&amp;T`  |`AT&T`   |  AT&T |
    |   |   |`&copy;`   |`&copy;`   |&copy;   |
    |   |   |`4 &lt; 5`   |`4 < 5`   | 4 < 5  |  

- HTML 코드로
    ```
    &nbsp;
    ```
    에 해당하는 공백을 다음 예제에서 카피하여 쓰면 편하다.
    >
    > 여기부터　　　　　여기까지

# 3. Setex-style 헤더 vs Atx-style 헤더
- Setex-style
    ```
    헤더 H1
    =  

    헤더 H2
    -  
    ```
- Atx-style
    ```
    ###### 헤더 H6
    ```

## 4. Blockquotes
- `>` before every line:
    ```
    > line............................................
    > line...............................................
    ```
- `>` before the first line:
    ```
    > long line ..................................................................................................................................................................................
    ```
- nested
    ```
    > First level
    > 
    >> Second level
    > 
    > Back to the first level
    ```
- headers, lists, and code blocks 들어간 블록.
    > # hello
    >
    >> - hi
    >> - greeting
    >
    >   ```python
    >   def bye():
    >       return "bye!"
    >   ```   
## 5. List
- `+`, `-`, `*`
1) one
1) two  
    1) two-one
    1) two-two  
        1. two-two-one
        3. two-two-two
* A list item paragraph

    blank line 은 `<p>` 테그와 같아서 단락을 만들어 준다. 

    `TAB`키 혹은 4`spaces`로 하나의 리스트 아이템에 paragraph를 구성할 수 있다. 

## 6. Links
- inline style
    ```
    [the delimited](link "optional title")
    ```  
- reference style: id는 대소구분없다.(NOT case-sensitive) 
    ```
    [the delimited][iD]
    ```
    and anywhere in the document,
    ```
    [Id]: link "optional title"
    ```
    Or up-to-three-spaces left margin, tab after colon`:`, using`()` instead of "" for title.
    ```
       [id]:   link (optional title)
    ```
    > implicit link name
    > ```
    > [Google][]
    > ```
    > ```
    > [Google]: http://google.com/
    > ```

## 7. Codes
- backtick `` ` `` by double backticks ( 전 ``` `` ```  후  ``` `` ```)

## 8. Images
- inline style 
    ```
    ![Alt text](/path/to/img.jpg "optional title")
    ```
- reference style
    ```
    ![Alt text][id]
    ```
    ```
    [Id]: url/to/image "optional title"
    ```
- Markdown에서는 이미지 조정기능이 없으므로 HTML의 기능을 차용해야 한다.
    
    ```md
    <img src="mylocalimg.jpg" alt="myimg" title="My Lovely Flowers" width="150" height="100" />
    ```
    More info in [stackoverflow][so]


## References

1. [JOHN GRUBER](https://daringfireball.net/projects/markdown/)
2. [Table Generator](https://www.tablesgenerator.com/markdown_tables)
3. [Stackoverflow][so]  
   
   [so]: https://stackoverflow.com/questions/255170/markdown-and-image-alignment#answer-5054055