# New Platform Interface Class Diagram

```plantuml

package MC #pink
{
abstract IGeneralFactory #header:red
{
  +createTasks() = 0
  +createObjects() = 0
}
class CFactory #header:red
{
  +createTasks()
  +createObjects()
  #Container <IGeneralMgr*> m_objContainer
}
abstract IGeneralMgr
abstract IPlatformMgr
class CPlatformMgr
abstract IHW1Mgr
{
  --public--
  struct SHW1Data
}
class CHW1Mgr
{
  --public--
  +CHW1MgrAdapter(IHW1MgrAdapter&)
  --protected--
  MBWriter<SHW1Data> hw1Writer
}
abstract IHW1MgrAdapter
class CHW1MgrAdapter
{
  --public--
  +doInit() = 0
  +doHandlePIDataIn() = 0
  +doHandlePIDataOut() = 0
}

abstract IPlatformInterface
{
  +doInit() = 0
  +doHandlePIDataIn() = 0
  +doHandlePIDataOut() = 0
}

IGeneralFactory <|-- CFactory
IGeneralMgr     <|-- IPlatformMgr
IPlatformMgr    <|-- CPlatformMgr
IGeneralMgr     <|-- IHW1Mgr
IHW1Mgr         <|-- CHW1Mgr
IHW1MgrAdapter  <|-- CHW1MgrAdapter
CHW1Mgr         o--> IHW1MgrAdapter
CFactory "1" o--> "*" IGeneralMgr

}

'''''''''''''''''''''''''''''''''
''' DEFINE PLATFORM INTERFACE '''
'''''''''''''''''''''''''''''''''
package PI #lightblue
{
class CPIFactory #header:red
{
  + createObjects()
}

abstract IPIComponentMgr
{
  --public--
  +doInit(IGeneralFactory*) = 0
  +doHandleDataIn(SFromPi*, const SToPi*) = 0
  +doHandleDataOut(SFromPi*, const SToPi*) = 0
}

class CPIMgr
{
  --public--
  +doInit()
  +doHandlePIDataIn()
  +doHandlePIDataOut()
  +AddComponent()
  --private--
  -IPIComponentMgr* m_data<b>In</b>Component [MAX_COMP_NUM]
  -IPIComponentMgr* m_data<b>Out</b>Component [MAX_COMP_NUM]
}

class CPIDataProcInMgr
{
  --public--
  +bool connectComponent(CPIDataProcInCommon*)
  +doInit(IGeneralFactory*)
  +doHandleDataIn(SFromPi*, const SToPi*)
  +doHandleDataOut(SFromPi*, const SToPi*)
  --protected--
  #CPIDataProcInCommon* m_dataProcIn
}

class CPIDataProcOutMgr
{
  --public--
  +bool connectComponent(CPIDataProcOutCommon*)
  +doInit(IGeneralFactory*)
  +doHandleDataIn(SFromPi*, const SToPi*)
  +doHandleDataOut(SFromPi*, const SToPi*)
  --protected--
  #CPIDataProcOutCommon* m_dataProcOut
}

class CPIDataProcInCommon
{
  --public--
  +doInit(IGeneralFactory*)
  +doHandleDataIn(SFromPi*, const SToPi*)
  +doHandleDataOut(SFromPi*, const SToPi*)

  +setDataProcInData(SFromPi*, const SToPi*)
  --protected--
  # performCommonLogicOperation1()
  # calculateCommonUsedData1()
}
note left of CPIDataProcInCommon::"performCommonLogicOperation1()"
functions that perform
common operation from 
the platform 
end note

class CPIDataProcOutCommon
{
  --public--
  +doInit(IGeneralFactory*)
  +doHandleDataIn(SFromPi*, const SToPi*)
  +doHandleDataOut(SFromPi*, const SToPi*)

  +setDataProcOutData(SFromPi*, const SToPi*)
  --protected--
  # performCommonLogicOperation1()
  # calculateCommonUsedData1()
}
note right of CPIDataProcOutCommon::"performCommonLogicOperation1()" 
functions that perform
common operation for 
the platform 
end note


class CPIDataProcInSpecific #lightgreen
{
  --public--
  +doInit(IGeneralFactory*)
  +doHandleDataIn(SFromPi*, const SToPi*)
  +doHandleDataOut(SFromPi*, const SToPi*)
  --protected--
  # setRxMsg1Data()
  # setRxMsg2Data()
}

class CPIDataProcOutSpecific #lightgreen
{
  --public--
  +doInit(IGeneralFactory*)
  +doHandleDataIn(SFromPi*, const SToPi*)
  +doHandleDataOut(SFromPi*, const SToPi*)
  --protected--
  # setTxMsg1Data()
  # setTxMsg2Data()
}

' Declare package class relations
CPIFactory *--> CPIDataProcInCommon
CPIFactory *--> CPIDataProcOutCommon


CPIMgr "1" o-l-> "*" IPIComponentMgr
IPIComponentMgr <|-- CPIDataProcInMgr
IPIComponentMgr <|-- CPIDataProcOutMgr
IPIComponentMgr <|-- CPIDataProcInCommon
IPIComponentMgr <|-- CPIDataProcOutCommon
CPIDataProcInCommon <|-- CPIDataProcInSpecific 
CPIDataProcOutCommon <|-- CPIDataProcOutSpecific 
CPIDataProcInMgr "1" o---> "1" CPIDataProcInCommon
CPIDataProcOutMgr "1" o---> "1" CPIDataProcOutCommon
}

' Cross package relations
CFactory   <|-- CPIFactory
IPlatformInterface <|-- CPIMgr
CPlatformMgr "1" o--> "*" CPIMgr
CPlatformMgr "1" *--> "1" IPlatformInterface
```
---


---

---
---
---
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