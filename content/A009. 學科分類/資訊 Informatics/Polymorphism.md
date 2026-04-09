---
cssclasses:
  - center-titles
tags:
  - informatics
  - programming
  - computer_science
aliases:
  - 多態
relevance: "[[Allotropy]]"
relevance 2: 
disambiguation: 
banner: 
media:
---
Date: 2024-08-12
File Creation Date: 2024-01-25 22:23
Last Modified: 2024-08-12 16:24
File folder: 資訊 Informatics
- ==(Object-oriented programming 物件導向程式語言)
	- 1 interface to differnt entities
- ==Types
	- Ad hoc polymorphism 特設多型
		- Ad hoc (After this) 後此
			- Ad hoc fallacy 後此謬誤
	- Parametric polymorphism 參數多型
		- Parameter 參數
	- Subtyping 子類型
	- Row polymorphism 行多型
	- Polytypism
	- Rank polymorphism 階級多型
	- Static polymorphism 靜態多型
	- Dynamic polymorphism 動態多型
	- Polymorphic code (for computer virus) 多型碼(電腦病毒用途)

##### Ad hoc polymorphism
Program Adhoc;

Function Add (x, y : Integer) : Integer;
Begin
    Add := x + y
End;

Function Add (s, t : String) : String;
Begin
    Add := Concat (s, t)
End;

Begin
    Writeln (Add (1, 2));                   (* Prints "3"             *)
    Writeln (Add ('Hello, ', 'Mammals!'));    (* Prints "Hello, Mammals!" *)
End.

##### Parametric polymorphism
- - - 
**Usual**
data List a = Nil | Cons a (List a)

length :: List a -> Integer
length Nil         = 0
length (Cons x xs) = 1 + length xs

map :: (a -> b) -> List a -> List b
map f Nil         = Nil
map f (Cons x xs) = Cons (f x) (map f xs)
- - - 
**C++, Delphi, Java, and GO**
data List a = Nil | Cons a (List a)

length :: List a -> Integer
length Nil         = 0
length (Cons x xs) = 1 + length xs

map :: (a -> b) -> List a -> List b
map f Nil         = Nil
map f (Cons x xs) = Cons (f x) (map f xs)
- - - 
##### Subtyping
abstract class Pet {
    abstract String speak();
}

class Cat extends Pet {
    String speak() {
        return "Meow!";
    }
}

class Dog extends Pet {
    String speak() {
        return "Woof!";
    }
}

static void letsHear(final Pet pet) {
    println(pet.speak());
}

static void main(String[] args) {
    letsHear(new Cat());
    letsHear(new Dog());
}
**PIC**
![](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e9/UML_class_pet.svg/529px-UML_class_pet.svg.png)