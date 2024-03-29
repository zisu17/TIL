# Spark 기초
## Spark Cluster
스파크 클러스터는 여러 대의 컴퓨터를 하나로 묶어 하나의 시스템처럼 사용할 수 있도록 하는 분산 시스템으로 스파크 애플리케이션을 실행하는 분산 환경이다.  
각 노드는 자신의 로컬 머신에 대한 자원을 제공하며, 이러한 자원들이 모여서 클러스터를 형성한다. 그 덕분에 대용량 데이터를 빠른 속도로 분산 처리할 수 있다. 스파크 클러스터는 마스터 노드와 워커 노드로 구성되어 있다.

## Spark Node
스파크 클러스터에서 노드는 클러스터를 이루는 개별적인 컴퓨터나 서버를 의미한다. 노드는 마스터 노드와 워커 노드로 나눠진다. 마스터 노드는 스파크 애플리케이션을 실행시키고, 작업을 조정하며 클러스터에서 실행중인 워커 노드들의 리소스를 관리하는 역할을 한다. 워커 노드는 실제로 작업을 실행하는 노드로 스파크 애플리케이션의 태스크들을 처리하고 데이터를 읽고 쓰며 스파크 애플리케이션의 실행을 도와주는 역할을 한다.

## Spark Worker
스파크 워커는 스파크 클러스터에서 실제로 데이터를 처리하는 단위이다. 각각의 워커는 클러스터에 있는 물리적인 머신이나 가상 머신에서 실행되며 노드에서 할당된 일부 CPU와 메모리를 사용하여 작업을 처리한다. 스파크 워커는 스파크 애플리케이션에서 RDD나 DataFrame과 같은 분산 데이터 구조를 생성하고 조작하며 클러스터의 다른 워커와 함께 작업을 수행하여 결과를 생성한다. 스파크 워커는 마스터 노드에서 할당된 작업을 수행하고 작업이 완료되면 마스터 노드에 결과를 보고한다.

## Spark에서 YARN의 역할
스파크는 클러스터 모드에서 YARN과 같은 클러스터 매니저를 사용하여 작업을 스케줄링하고 클러스터의 자원을 효율적으로 활용할 수 있다. YARN은 Hadoop의 서브프로젝트로 클러스터 내에서 리소스 관리와 스케줄링을 담당하는 프레임워크이다. 스파크는 YARN 위에서 실행될 때 YARN의 리소스 매니징 및 스케줄링 기능을 이용하여 클러스터 내의 자원을 최적으로 활용할 수 있다. YARN을 사용하면 스파크 애플리케이션이 클러스터에 분산되어 실행되며 각 노드에서 스파크 워커가 실행된다. 이렇게 스파크 애플리케이션의 처리 속도와 효율성이 크게 향상될 수 있다.