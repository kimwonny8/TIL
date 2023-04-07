## Branch의 종류

**1. master 브랜치** **(제품으로 출시될 수 있는 브랜치)**

사용자에게 배포 가능한 상태만을 관리한다. 배포(release) 이력을 관리하기 위해 사용한다.

즉, 함부로 master 브랜치에 병합(merge)하게 되면 안된다. 항상 master 브랜치에서 작업하고 있는 건 아닌지 확인해야 된다.

 

**2. develop 브랜치 (다음 출시 버전을 개발하는 브랜치)**

기능 개발을 위한 브랜치들을 병합하기 위해 사용한다. 즉, 모든 기능이 추가되고 버그가 수정되어 배포 가능한 안정적인 상태라면 develop 브랜치를 master 브랜치에 병합한다.

 

**3. feature 브랜치 (기능을 개발하는 브랜치)**

feature 브랜치는 새로운 기능 개발 및 버그 수정이 필요할 때마다 develop 브랜치로부터 분기한다.

feature 브랜치에서의 작업은 기본적으로 공유할 필요가 없기 때문에 자신의 로컬 저장소에서 관리한다.

개발이 완료되면 develop 브랜치로 merge하여 다른 사람들과 공유한다.

- develop 브랜치에서 새로운 기능에 대한 feature 브랜치를 분기한다.
- 새로운 기능에 대한 작업을 수행한다.
- 작업이 끝나면 develop 브랜치로 merge 한다.
- 더 이상 필요하지 않은 feature 브랜치는 삭제한다. - 많은 줄기(branch)는 작업에 혼란을 줄 수 있다.
- 새로운 기능에 대한 feature 브랜치를 중앙 원격 저장소에 올린다.



**4. release 브랜치 (이번 출시 버전을 준비하는 브랜치)**

배포를 위한 전용 브랜치를 사용함으로써 한 팀이 해당 배포를 준비하는 동안 다른 팀은 다음 배포를 위한 기능 개발을 계속할 수 있다. 1팀은 1.2버전을 개발하고, 2팀은 1.3버전을 개발할 수 있다.

release 브랜치는 배포를 위한 최종적인 버그 수정, 문서 추가 등 배포와 직접적으로 관련된 작업을 수행한다. 이러한 작업들 이외에 release 브랜치에 새로운 기능을 추가로 merge하지 않는다.

 

**5. hotfix 브랜치 (출시 버전에서 발생한 버그를 수정하는 브랜치)**

배포한 버전에 긴급하게 수정을 해야 할 필요가 있을 경우, master 브랜치에서 분기하는 브랜치이다. develop 브랜치에서 문제가 되는 부분을 수정하여 배포 가능한 버전을 만들기에는 시간도 많이 소요되고 안정성을 보장하기도 어려우므로 바로 배포가 가능한 master 브랜치에서 직접 브랜치를 만들어 필요한 부분만 수정한 후 다시 master 브랜치에 병합하여 이를 배포하는 것이다.

- 배포한 버전에 긴급하게 수정할 부분 발생 => master브랜치에서 hotfix브랜치 분기
- 문제가 되는 부분 수정후, master브랜치에 merge하고 배포
- hotfix 브랜치에서의 변경 사항은 develop 브랜치에도 merge한다.

###  

## Branch 네이밍 규칙

**1. master branch, develop branch**

master와 develop 브랜치는 본래 이름 그대로 사용하는 경우가 일반적이다.

**2. feature branch**

- 어떤 이름도 가능하다. 단, master, develop, release-..., hotfix-... 같은 이름은 사용할 수 없다.
- feature/기능요약 형식을 추천한다. ex) feature/home
- feature/{issue-number}-{feature-name} 이슈추적을 사용한다면 이와 같은 형식을 따른다.
  ex) feature/1-home-markup, feature/2-build-header

**3. release branch**

- release-RB_... 또는 release-... 또는 release/...같은 이름이 일반적이다.
- release-... 형식을 추천한다. ex) release-1.7

**4. hotfix branch**

- hotfix-... 형식을 추천한다. ex) hotfix-1.4.1