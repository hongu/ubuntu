# RFID Protocol #

  <img src="https://github.com/hongu/etc/blob/master/struct.png" width="500" height="500">


<br /><br />
**1. PACKET FORMAT**
----------
> 기본 데이터 포맷은 3개의 필드로 구성

|      | Header | Data | Trailer |
|:--------|:--------:|:--------:|:--------:|
| byte | 1 | Variable | 2 |
| HEX | 0x3E |  | 0x0D, 0x0A |
| ASCII | '>' |  | '\r', '\n' |

<br /><br />
**2. PACKET CONNECTION**
----------
> 리더 연결 확인을 위한 패킷   
* Connection Confirm Command (Host -> Reader )

|      | Header | Command Code | Trailer |
|:--------|:--------:|:--------:|:--------:|
| byte | 1 | 1 | 2 |
| HEX | 0x3E | 0x63 | 0x0D 0x0A | 
| ASCII | > | c | \r \n |

* Connection Reply ( Reader -> Host )

|      | Header | Command Code | Trailer |
|:--------|:--------:|:--------:|:--------:|
| byte | 1 | 1 | 2 |
| HEX | 0x3E | 0x6E | 0x0D 0x0A | 
| ASCII | > | n | \r \n |

<br /><br />
**3. SET CONTROL COMMAND**
----------
>리더 환경 설정을 위한 패킷      
              
|      | Header | Command Code | SP | Control Code | SP | Value | Trailer |
|:--------|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|
| byte | 1 | 1 | 1 | 1 | 1 | Variable | 2 |
| HEX | 0x3E | 0x78 | 0x20 | | 0x20 | | 0x0D 0x0A | 
| ASCII | > | x |  |  |  | | \r \n |
```
ex) >x c 1\r\n
```

<br /><br />
**4. GET CONTROL COMMAND**
----------
>리더의 환경 설정값을 읽어오는 패킷     

* Get Control Command

|      | Header | Command Code | SP | Control Code | Trailer |
|:--------|:--------:|:--------:|:--------:|:--------:|:--------:|
| byte | 1 | 1 | | Variable | 2 |
| HEX | 0x3E | 0x79 | 0x20 | | 0x20 | | 0x0D 0x0A | 
| ASCII | > | y |  |  | \r \n |
```
ex) >y c\r\n
```

* Get Control Reply

|      | Header | Control Code | SP | Control Value | Trailer |
|:--------|:--------:|:--------:|:--------:|:--------:|:--------:|
| byte | 1 | 1 | | Variable | 2 |
| HEX | 0x3E | 0x79 | 0x20 | | 0x20 | | 0x0D 0x0A | 
| ASCII | > |  |  |  | \r \n |
```
ex) >c 1\r\n
```

<br /><br />
* Command Code list

| No. | Code(ASCII) | Code(HEX) | Name | Value | Default |
|:--------|:--------:|:--------:|:--------:|:--------:|:--------:|
| 1 | b | 0x62 | baud rate  | 0 ~ 3 | 0(9600bps) | 
| 2 | c | 0x63 | Continuous Mode | 0, 1 | 1 | 
| 3 | d | 0x64 | All default Set |  |  |
| 4 | p | 0x64 | Power Gain | 0 ~ (최대치)  | |
| 5 | r | 0x72 | Tag Report Time | 100 ~ 2000 | 500(ms) |  

<br /><br />
* Baud Rate : 리더의 UART Baud Rate 를 설정한다

| Value | Baud Rate |
|:--------|:--------:|
| 0 | 9600 bps |
| 1 | 19200 bps |
| 2 | 38400 bps |
| 3 | 115200 bps |

<br /><br />
* All default set :  모든 환경설정 값을 Default로 설정한다.

<br /><br />
* Power Gain :  리더의 RF 파워 레벨을 변경 한다.
( 첨부한 문서의 설정값을 참조 하시기 바랍니다.)

<br /><br />
* Tag Report Time : Coutinous 모드로 동작 할 경우 전송 주기 설정

| Default Report Time | Range |
|:--------|:--------:|
| 500 (ms) | 100 ~ 2000 (ms) |


<br /><br />
**5. READER COMMAND PACKET**
----------
>리더의 TAG ID 값을 읽어오거나 쓸때 사용하는 명령어
              
|      | Header | Command Code | SP | Parameter | Trailer |
|:--------|:--------:|:--------:|:--------:|:--------:|:--------:|
| byte | 1 | 1 | 1 | Variable | 2 |
| HEX | 0x3E |  | 0x20 |   | 0x0D 0x0A | 
| ASCII | > |  |  |  | \r \n |

```
ex) >w 3F13DA3300000000\r\n
```

* Reader Command list

| No. | Code(ASCII) | Code(HEX) | Name | 
|:--------|:--------:|:--------:|:--------:|
| 1 | f | 0x66 | Inventory | 
| 2 | r | 0x72 | Memory Read | 
| 3 | w | 0x77 | Memory Write | 
| 4 | s | 0x73 | Inventory Stop |

<br /><br />
* Inventory 

> Tag 의 Data 를  연속적으로 읽어오는 명령어 

> 리더는 Inventory 실행하여 Tag 가 Detect 되었을 경우 응답을 한다.

##### Inventory Start Command

| No. | Header | Command Code | Trailer | 
|:--------|:--------:|:--------:|:--------:|
| Length(byte) | 1 | 1 | 2 | 
| Hex | 0x3E | 0x66 | 0x0D 0x0A |
| ASCII | > | f | \r\n |

```
ex) >f\r\n 
```

##### Data Reply

| No. | Header | Reply Code | Data | Checksum | Trailer | 
|:--------|:--------:|:--------:|:--------:|:--------:|:--------:|
| Length(byte) | 1 | 1 | variable | 2 | 2 |
| Hex | 0x3E | 0x54 |  | | 0x0D 0x0A | 
| ASCII | > | T | | | \r\n |

```
※ Checksum 은 Data 필드를 1byte 씩 Exclusive OR(^) 한 값의 Ascii 변환한 값이다.

 Ex) Data : 0x30^0x31^0x32^0x33^0x34 = 0x34 이므로 Ascii 로 변환하면 ’3’(0x33), ’4’(0x34) 2byte 가 된다
```

<br /><br />

* Memory Read

> Tag 의 Data를  한번만 읽어오는 명령어 

> Data Replay는  Inventory 와 동일

##### Memory Read Command

| No. | Header | Command Code | Trailer | 
|:--------|:--------:|:--------:|:--------:|
| Length(byte) | 1 | 1 | 2 | 
| Hex | 0x3E | 0x66 | 0x0D 0x0A |
| ASCII | > | r | \r\n |

<br /><br />
* Memory Write

> Tag 의 Memory 를 직접 Access 하여 Write 하기 위한 명령어

##### Memory Write Command

| No. | Header | Command Code | SP | Data | Trailer | 
|:--------|:--------:|:--------:|:--------:|:--------:|:--------:|
| Length(byte) | 1 | 1 | 1 | Variable | 2 | 
| Hex | 0x3E | 0x77 | | |0x0D 0x0A |
| ASCII | > | w | | | \r\n |

```
ex) >w 3F13DA3300000000\r\n
```

##### Data Reply

> 응답은 Write한 데이터를 반환

| No. | Header | Command Code | SP | Data | Trailer | 
|:--------|:--------:|:--------:|:--------:|:--------:|:--------:|
| Length(byte) | 1 | 1 | 1 | Variable | 2 | 
| Hex | 0x3E | 0x43 | | | 0x0D 0x0A | 
| ASCII | > | C | | | \r\n |

```
ex) >C 3F13DA3300000000\r\n
```


<br /><br />
* Inventory Stop

> 실행된 Inventory를 중지 시킨다.

##### Inventory Start Command

| No. | Header | Command Code | Trailer | 
|:--------|:--------:|:--------:|:--------:|
| Length(byte) | 1 | 1 | 2 | 
| Hex | 0x3E | 0x73| 0x0D 0x0A |
| ASCII | > | s | \r\n |

```
ex) >s\r\n 
```
