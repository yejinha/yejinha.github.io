---
layout: post
title:  프로그래머스 문제 풀이 1 -  완주하지 못한 선수 (Java)
date:  2022-02-15 15:05
img:  # Add image post (optional)
categories: [codingtest]
tags: [IT,  JAVA, algorithm, hashmap] # add tag
sitemap :
changefreq : daily
priority : 1.0
---

코테 문제 풀이 한 것도 하나씩 올려보고자 합니당.    
스스로 복기할 겸 + 검색해 들어오신 분들에게 조금이라도 도움되었으면 합니다.  

*문제 내용*
---
수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.

마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.

제한사항
마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.
completion의 길이는 participant의 길이보다 1 작습니다.
참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.
참가자 중에는 동명이인이 있을 수 있습니다.
입출력 예
participant	completion	return
["leo", "kiki", "eden"]	["eden", "kiki"]	"leo"
["marina", "josipa", "nikola", "vinko", "filipa"]	["josipa", "filipa", "marina", "nikola"]	"vinko"
["mislav", "stanko", "mislav", "ana"]	["stanko", "ana", "mislav"]	"mislav"

---

~~~java
        public static String solution(String[] participant, String[] completion) {
            String answer = "";
            
            /* 풀이 과정 
             지원자 - 완주자를 비교하여 중복된 값을 제거하고 지원자에 남은 값이 완주 못한 자입니다.   
             중복된 키 값을 허용하지 않는 hashmap의 특성을 사용해 풀었습니다. 
             1) 지원자를 모두 hashmap 에 이름 키 값으로 넣음.   
             이때 value 는 이름이 들어간 횟수로 하였습니다. (중복 이름 가능성때문에)  
             2) 완주자 리스트 돌면서 지원자 hashmap에 검색되면 삭제함.  
             중복 이름 때문에 value 가 2 이상(두 명 리스트에 있음) 이라면 value 값을 줄여 카운트 처리만 하고 삭제하지 않았습니다. 
             3) 완주자 돌면서 삭제하고 남은 값 하나가 완주 못한자이기에 키 값 뽑아서 answer 에 넣도록 함. 
             */
        
            //value 로 쓸 변수, hashmap 초기화 
            int index = 0;
            HashMap<String,Integer> answerMap=new HashMap<>();

            // 지원자 돌면서 이미 있다면 value +1,  없다면 1 로 맵에 넣기 (1)
            for(String name : participant){
                if(answerMap.containsKey(name)){
                    index = answerMap.get(name);
                    answerMap.put(name, ++index);
                }else{
                    answerMap.put(name, 1);
                }
            }

            // 완주자 돌면서 키 있고, 값 1 이라면 바로 삭제,  값 2 이상이면 값 - 1 로 교체 
            for(String completedName : completion){
                if(answerMap.containsKey(completedName)){
                    index = answerMap.get(completedName) ;
                    if(index == 1 ){
                        answerMap.remove(completedName);
                    }else if(index > 1){
                        answerMap.put(completedName,--index);
                    }
                }else{  // key 값 업다면 바로 이름 넣기 
                    answer = completedName;
                }
            }

           if(answer.equals("")){  //키 없는 값 있어서 answer 정해지지 않았다면 
                List<String> list =new ArrayList<>(answerMap.keySet());  
                answer = list.get(0);
            }
           
            return answer;


~~~