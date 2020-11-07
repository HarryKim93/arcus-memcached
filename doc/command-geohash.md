# GEO 
- Sorted Set Data Structure를 사용하여 구현한다.
- \<value>, \<longitude>, <latitude\> 로 구성된 데이터 집합을 value로 가지는 item을 가진다.

# GEO 명령

- [Geo collection 생성: gop create]
Geo element에 관한 명령은 아래와 같다.

- [Geo element 삽입: gop add]
- [Geo element 변경: gop update]
- [Geo element 삭제: gop delete]
- [Geo element 조회: gop pos]
- [Geo element 간 거리 조회: gop dist]
- [Geo element 근처에 있는 geo element 조회: gop raditem]
- [지정 좌표 근처에 있는 geo element 조회: gop radlocation]
- [Geo hash 조회: gop hash]

### gop create (Geo Collection 생성)

Geo collection을 empty 상태로 생성한다.

```
gop create <key> <attributes> <hashlen> [noreply]\r\n
* <attributes>: <flags> <exptime> <maxcount> [<ovflaction>] [unreadable]
```

- \<key\> - 대상 item의 key string
- \<attributes\> - 설정할 item attributes.
- \<hashlen> - 설정할 geo hash의 길이
- noreply - 명시하면, response string을 전달받지 않는다.

### gop add (Geo Element 삽입)

Geo collection에 하나의 field, element를 삽입한다.
Geo collection을 생성하면서 \<field, value\>로 구성된 하나의 element를 삽입할 수도 있다.

```
gop add <key> <field> <bytes> [create <attributes>] [noreply|pipe]\r\n
[<longitude> <latitude>]\r\n
* <attributes>: <flags> <exptime> <maxcount> [<ovflaction>] [unreadable]
```

- \<key\> - 대상 item의 key string
- \<field\> - 삽입할 element의 field string
- \<bytes\> - 삽입할 element의 데이터 길이 (trailing 문자인 "\r\n"을 제외한 길이)
- create \<attributes\> - 해당 Geo collection 없을 시에 Geo collection 생성 요청.
- noreply or pipe - 명시하면, response string을 전달받지 않는다.
- [\<longitude> \<latitude>] - 삽입할 경도, 위도

### gop update (Geo Element 변경)

Geo collection에서 하나의 field에 대해 element 변경을 수행한다.
현재 다수 field에 대한 변경연산은 제공하지 않는다.

```
gop update <key> <field> <bytes> [noreply|pipe]\r\n
[<longitude> <latitude>]\r\n
```

- \<key\> - 대상 item의 key string
- \<field\> - 삽입할 element의 field string
- \<bytes\> - 변경할 데이터 길이 (trailing 문자인 "\r\n"을 제외한 길이)
- noreply or pipe - 명시하면, response string을 전달받지 않는다.
- [\<longitude> \<latitude>] - 삽입할 경도, 위도

### gop delete (Geo Element 삭제)

Geo collection에서 하나 이상의 field 이름을 주어, 그에 해당하는 element를 삭제한다.

```
gop delete <key> <field> [drop] [noreply|pipe]\r\n
```

- \<key\> - 대상 item의 key string
- drop - field, element 삭제로 인해 empty geo가 될 경우, 그 Geo을 drop할 것인지를 지정한다.
- noreply or pipe - 명시하면, response string을 전달받지 않는다.

### gop pos (Geo Field, Element 조회)

Geo collection에서 하나 이상의 field 이름을 주어, 그에 해당하는 element를 조회한다.

```
gop pos <key> <field> [delete|drop]\r\n
```

- \<key\> - 대상 item의 key string
- \<field\> - 삽입할 element의 field string
- delete or drop - field, element 조회하면서 그 field, element를 delete할 것인지,
그리고 delete로 인해 empty geo가 될 경우 그 geo를 drop할 것인지를 지정한다.

### gop dist (두 Geo element 간의 거리 조회)

Geo collection에서 두 개의 field 이름을 주어, 두 element 사이의 거리를 조회한다.

```
gop dist <key> <field> <field> <unit>\r\n
```

- \<key\> - 대상 item의 key string
- \<field\> - 삽입할 element의 field string
- \<unit\> - 반경의 단위 (m, km, ft, mi)

### gop raditem (Geo element 근처에 있는 Geo element 조회)

Geo collection에서 field 이름과 raidus 값을 주어, field에 해당하는 element에서 반경이내의 element를 조회한다.

```
gop raditem <key> <field> <radius> <unit>\r\n
```

- \<key\> - 대상 item의 key string
- \<field\> - 삽입할 element의 field string
- \<radius\> - element로부터 조회할 반경의 거리
- \<unit\> - 반경의 단위 (m, km, ft, mi)

### gop radlocation (입력한 좌표 근처에 있는 Geo element 조회)

Geo collection에서 하나 field이름과 radius

```
gop radlocation <key> <longitude> <latitude> <radius> <unit>\r\n
```

- \<key\> - 대상 item의 key string
- \<longitude\> - 조회할 location의 경도
- \<latitude\> - 조회할 location의 위도
- \<radius\> - element로부터 조회할 반경의 거리
- \<unit\> - 반경의 단위 (m, km, ft, mi)

### gop hash (Geo element의 해시값 조회)

Geo collection에서 하나 이상의 field 이름을 주어, 그에 해당하는 element를 조회한다.

```
gop hash <key> <field>\r\n
```

- \<key\> - 대상 item의 key string
- \<field\> - 삽입할 element의 field string