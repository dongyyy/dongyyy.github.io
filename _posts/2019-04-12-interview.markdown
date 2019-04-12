---
layout: post
title:  "메소드 시그니처 (Method signature)"
date:   2019-04-11 21:20:32
author: Dongy
categories: Java
cover:
---

## 메소드 시그니처 (Method signature)
<br>
<strong>컴파일러가 메소드를 구분할 수 있게 해주는 서명(Signature)입니다.</strong><br>
<br>
자바에서는 <span style="color:red">메소드의 이름과 파라미터 </span>만을 메소드의 시그니처 (Method Signature) 라고 합니다.<br>
<br>
아래 코드를 살펴보면, <br>
draw() 메소드들은 이름은 같지만 <span style="color:red">파라미터의 갯수와 타입<span>이 다릅니다.<br> 
따라서 컴파일러가 서로 다른 메소드로 구분합니다.<br>
<br>


### Code
```

public class Pen {
    
    public void draw(String s) {
        //…
    }

    public void draw(int i) {
        //…
    }

    public void draw(double f) {
        //…
    }

    public void draw(int i, double f) {
        //…
    }

    //위 draw 메소드와 시그니처가 같아 선언 불가능
    //public int draw(int i, double f) {
    //    //…
	//  return 0;
    //}

}

```


단, 리턴타입은 메소드 시그니처에 포함되지 않습니다.<br>
따라서 컴파일러는 메소드를 구분할 때 리턴 타입을 고려하지 않기 때문에 <br>
서로 다른 리턴 타입을 가져도 동일한 시그니처를 가진 2개의 메소드를 선언할 수 없습니다.<br>
<br><br>


### Java 기본 문법의 중요성,,,
어제 한 기업 안드로이드 개발자로 최종면접을 봤습니다.<br><br>
감사하게도 면접관님들께서 편안한 분위기로 면접을 진행해주셨습니다.<br><br>
하지만,,, 대부분의 질문이 자바 기본 문법에 대한 질문이었고<br>
스스로에게 부끄러울만큼 답변을 제대로 하지 못했습니다.<br><br>
앞으로는 IDE에서 알아서 체크해주고 API에서 처리해주고 프레임웍 차원에서 지원해줘서 사용하지 않았던 Java 기본 개념들에 대해서도 포스팅할 계획입니다. <br><br>
