```plantuml

A <.. B
note on link 
<b>Dependency<b> 
class b is using class B
end note
```
```plantuml
Animal <|-- Dog
note on link 
<b>Inheritance<b> 
A Dog is an instance of Animal.
The takes some Animal attributions and actions.
end note
```

```plantuml
Car x-->"4..10" CarWheel
note on link 
<b>Association<b> 
the Car has a connection to a Wheel
a Car can have 4 to 10 Wheels connected to it hance
the arrow direction
The "x" states that a Wheel doesn't have a reference to the Car.
end note
```

```plantuml
Human o--> Glasses
note on link 
<b>Aggregation<b> 
A Human can own a pair of glasses.
The Glasses class is created from the Human
This connection a <u>week</u> as not all Humans needs Glasses
end note
```

```plantuml
Human "1"*-->"1" Hart
note on link 
<b>Composition<b> 
The Hart is a part, what compose a Human.
This connection a <u>strong</u> as a Human can't live without a Hart
A Human has only 1 Hart.
A Hart can belong to only 1 Human.
end note
```