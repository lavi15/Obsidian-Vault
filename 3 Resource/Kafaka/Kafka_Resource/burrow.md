
- lag의 실시간 확인이 필요할 경우 ElasticSearch, InfluxDB에 넣어 Grafana로 확인 가능 
- 다만 컨슈머 단계에서 모니터링 하는 것은 운영요소가 많이 들어감(컨슈머에 DI)
- 이에 kafka를 모니터링 할 수 있는 burrow를 사용

### Burrow의 특징
- 멀티 카프카 클러스터 지원 : 카프카 클러스터가 여러개여도 하나의 burrow 모니터링 가능
- sliding window를 통한 Consumer의 status 확인 : Error, Warning, ok 로 상태 확인 가능 
- HTTP api 제공 