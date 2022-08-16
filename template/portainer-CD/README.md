
# portainerCD


## example)

stacks 에 yml 스택을 정의한다.
- node application
- nginx sidecard

yml 환경 구성 파일을 git에 저장한다.
- git은 push가 발생하면 webhook을 날린다.
- 혹은 polling으로 업데이트를 인지한다.

portainer을 이용해서 
- 베포가 완료되면, tag정보를 git에 업데이트하여
- portainer로 자동 베포 하게끔 만든다.

