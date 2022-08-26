# docker-deployment

## Goal

- docker 컨테이너 관리


## CI Process

1. 로컬 커밋 이후 원격 코드 저장소에 푸쉬가 발생 (master 브랜치 등)
2. jenkins가 이를 인지(polling or webhook)
3. 새로운 형상을 가져와서 빌드를 시작 ( or jenkins 웹에서 수동으로 시작 )
4. pipeline 빌드, 테스트, docker hub에 푸쉬  
5. deployment stack 형상을 변경  

## CD Process

0. portainer를 설치
1. deployment stack 레포를 구독  
2. 구독중인 docker-compose.yml 변경되면   
3. 스택에 맞게 재베포

## node


## stacks 

- 베포할 컨테이너를 docker-compose 파일로 명세.  
- 노드의 portainer을 통해 stack을 선택 후 베포.  