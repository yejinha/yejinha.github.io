---
layout: post
title:  해시 테이블 알고리즘, 설명과 java 구현, java collection framework hashmap 사용법 
date:  2022-02-01 18:05
img:  # Add image post (optional)
categories: [study]
tags: [IT,  JAVA, hashalgorithm] # add tag
sitemap :
changefreq : daily
priority : 1.0
---

작은 거라도 TIL 정리하기로 함 !!   

코테 문제 풀때 기본기가 없으니 자신이 없는거 같아서 어차피 면접 대비도 할 겸 겸사 겸사 정리한다.  

[엔지니어 대한민국님 해시테이블 유튜브 참고](https://www.youtube.com/watch?v=Vi0hauJemxA)

## 1. 해시 테이블 알고리즘  

key 를 입력 받아 hash 함수를 돌려 hash code 를 만든다. 
이 hashcode 를 테이블 index 로 환산해 저장한다. 
key, value  가 있는 값을 저장할 때 사용한다 !! 

### *장점*  
해시코드 자체가 인덱스를 만들때 사용되기에 검색할 필요 없어 조회가 빠르다.  

### *주의할 점* 
대신 인덱스 를 만드는 hash 알고리즘이 좋지 않을 경우 , 배열 한 칸에 너무 많은 데이터 들어가야하기에 collision 이 일어난다.   

충돌 많은 경우 O(N) 까지 복잡도가 높아질 수 있다.   

- 서로 다른 키로 같은 해시 코드를 가질 수 도 있음 
- 서로 다른 코드가 배열 방 한정되어있기에 같은 인덱스로 같은 방 들어갈 수도 있음. 

이런 걸 모두 collision 이라고 한다.   


## 2. java 로 구현한 코드 (MAP 컬렉션 없이)
~~~java
public class HashTableEx {

    //key, value 저장할 node 생성
    public class Node{
        private String key;
        private String value;
        Node(String key, String value){
            this.key=key;
            this.value=value;
        }

        //get, set 함수 생성
        public void setValue(String value){
            this.value= value;
        }
        public String getValue(){
           return this.value;
        }

    }

    private LinkedList<Node>[] list ;

    // 테이블 크기 초기화
    HashTableEx(int size){
        this.list = new LinkedList[size];
    }


    public int keyToHash(String key){
        int hash=0;
        char[] keyToChars = key.toCharArray();
        for(char c:keyToChars){
            hash+=(int)c;
        }
        return hash;
    }

    // % 3  연산,  크기 3개로 하였기에 이 인덱스 벗어나지 않도록
    public int hashToIndex(int hash){
        int index = hash % 3 ;
        return index;
    }

    public void put(String key, String value){
        int hash= keyToHash(key);
        int index =  hashToIndex(hash);
        // key to hash -> hash to index 거쳐서 인덱스 분배함.

        //들어갈 방 인덱스로 세팅
        LinkedList<Node> putList = list[index];
        //들어갈 방에 아무도 없으면 방에 linkedlist  초기화 하고 add
        if(putList ==null){
            putList = new LinkedList<Node>();
            list[index]=putList;
        }
        // key 는 중복 될 수 없기에 같은 키 있으면 value 바꾸기
        Node findNode = get(key);
        if(findNode == null){
            putList.add(new Node(key,value));
        }else{
            findNode.setValue(value);
        }
    }

    public Node get(String key){
        int hash= keyToHash(key);
        int index =  hashToIndex(hash);
        LinkedList<Node> findList = list[index];
        // key -> hash -> index 로 환산한 방 돌면서 key 찾기
        for(Node node : findList){
            if(node.key.equals(key)) return node;
        }
        return null;
    }

    public static void main(String[] args) {
        HashTableEx example = new HashTableEx(3);
        example.put("study","hard");   
        example.put("play","more harder");  
        example.put("eat","much more happier");  
        example.put("play", "^^");  

        System.out.println("study  " + example.get("study").value);
        System.out.println("play   " + example.get("play").value);
        System.out.println("eat    "+ example.get("eat").value);

    }
}


~~~

## 3. HashMap 컬렉션으로 구현  
친절한 자바는 이걸 다 구현해놔서 이렇게 사용하면 된다 !!! 


~~~java
Map<String, String> findByMap = new HashMap<>();
        findByMap.put("study","hard");
        findByMap.put("play","more harder");
        findByMap.put("eat","much more happier");
        findByMap.put("play", "^^");

        System.out.println("---------- use HashMap-------");
        System.out.println("study  " + findByMap.get("study"));
        System.out.println("play   " + findByMap.get("play"));
        System.out.println("eat    "+ findByMap.get("eat"));

~~~