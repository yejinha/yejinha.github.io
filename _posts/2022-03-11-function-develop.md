---
layout: post
title:  프로그래머스 문제 풀이 2 -  기능 개발(Java)
date:  2022-03-11 11:05
img:  # Add image post (optional)
categories: [codingtest]
tags: [IT,  JAVA, algorithm] # add tag
sitemap :
changefreq : daily
priority : 1.0
---

그래도 삶은 계속된당.   
오늘 면접본거 복기하기 전에 머리가 어지러워서 한 문제 풀었다.   
노좌절 킵고잉  

## 문제 내용 
프로그래머스 팀에서는 기능 개선 작업을 수행 중입니다. 각 기능은 진도가 100%일 때 서비스에 반영할 수 있습니다.

또, 각 기능의 개발속도는 모두 다르기 때문에 뒤에 있는 기능이 앞에 있는 기능보다 먼저 개발될 수 있고, 이때 뒤에 있는 기능은 앞에 있는 기능이 배포될 때 함께 배포됩니다.

먼저 배포되어야 하는 순서대로 작업의 진도가 적힌 정수 배열 progresses와 각 작업의 개발 속도가 적힌 정수 배열 speeds가 주어질 때 각 배포마다 몇 개의 기능이 배포되는지를 return 하도록 solution 함수를 완성하세요.

### 제한 사항
작업의 개수(progresses, speeds배열의 길이)는 100개 이하입니다. 
작업 진도는 100 미만의 자연수입니다.  
작업 속도는 100 이하의 자연수입니다.  
배포는 하루에 한 번만 할 수 있으며, 하루의 끝에 이루어진다고 가정합니다. 예를 들어 진도율이 95%인 작업의 개발 속도가 하루에 4%라면 배포는 2일 뒤에 이루어집니다.   
### 입출력 예  
progresses	speeds	return   
[93, 30, 55]	[1, 30, 5]	[2, 1]  
[95, 90, 99, 99, 80, 99]	[1, 1, 1, 1, 1, 1]	[1, 3, 2]   

---
스택/큐로 분류되어있지만 굳이 그거 쓸 필요 없다.  
FIFO 여서 그렇게 해놓은거 같은데 잘 모르겠음.   

내가 생각한 순서.   
1) 작업 완료에 걸리는 일수만이 중요함.  
- 일단 모든 작업에 걸리는 일수를 계산함.
2) 앞에서부터 비교하면서 나보다 작은 일수 필요하면 같이 배포, 큰 일수 필요하면 같이 배포 못함.  
- 최대값 찾는 것처럼 비교. input progresses 크기만큼만 한번만 돌면됨.  
3) 같이 배포 못하는 인덱스 찾으면 여태까지 같이 배포한 갯수 저장, 배포갯수 초기화  
4) 마지막 배포건은 갯수 저장하는 조건에 안걸리기에 루프 종료후 저장한번 해줌. 

~~~java
import java.util.*;

class Solution {
    public int[] solution(int[] progresses, int[] speeds) {
        int[] answer = {};  //주어진 리턴 타입 
        int[] neededDays = new int[progresses.length]; //모든 일의 각 필요일수 계산
        List<Integer> list = new ArrayList<>();  // 답 저장할 arraylist
                                                 //-> 배포 몇 번 있을지 모르니 고정 배열 쓸 수 없음. 
        
        for(int i =0; i<progresses.length; i++){
            neededDays[i] = (int)Math.ceil((double)(100-progresses[i])/(double)speeds[i]);  //올림으로 계산
        }
        
        int release =0;  //한번에 배포하는 갯수
        int index = 0 ; // 비교할 기준 값
        
        for(int i=0; i<progresses.length; i++){
            if(neededDays[index]>=neededDays[i]) {
                ++release;
            }else{ 
                list.add(release); //값 저장 
                release=1; //값 초기화 
                index=i; //기준값 옮기기 
            }
        }
        
        // 배열의 마지막 값까지 확인한 내용 넣음 (마지막 배포 개수)
        list.add(release);
        
        answer = new int[list.size()]; //정답 포맷에 맞게 변환
    
        for(int i =0 ; i<answer.length; i++){
            answer[i] =list.get(i);
        }
        
        return answer;
    }
}

~~~

