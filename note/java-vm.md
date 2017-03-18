# Java vm

GC방식
- Serial GC, Parallel GC, CMS GC, G1

paraller GC의 경우 HeapSize가 크면 GC가 오래걸린다고 함
64비트 OS and JVM일 경우, CMS, G1 GC를 사용하면 2G이상의 Heap memory 사용 가능

메모리 크기 및 GC옵션
- Xms, Xmx, NewRatio, NewSize:
- SurvivorRatio: Eden 영역과 Survivor 영역의 비율

## GC Allocation Failure

> "Allocation Failure" is a cause of GC cycle to kick.
> "Allocation Failure" means that no more space left in Eden to allocate object. So, it is normal cause of young GC.
> Older JVM were not printing GC cause for minor GC cycles.
> "Allocation Failure" is almost only possible cause for minor GC. Another reason for minor GC to kick could be CMS remark phase (if +XX:+ScavengeBeforeRemark is enabled).

## G1 GC

현재까지 parallel gc를 사용하고 있는데, 별 다른 문제점 (STW)이 없어서 그냥 쓰고 있다.

찾아본 문제점

- Humonogous object라고 불리는 메모리 사용량이 큰 객체에 대한 처리는 아직 최적화 되지 않음
- full gc를 피하기 어렵다. (이건 뭐 상대적인듯)

Spark의  old generation region에 대한 GC옵션 <https://databricks.com/blog/2015/05/28/tuning-java-garbage-collection-for-spark-applications.html> 자료가 너무 오래전이지 않을까...

```
-XX:+UseG1GC -XX:+UnlockDiagnosticVMOptions -XX:+G1SummarizeConcMark -XX:InitiatingHeapOccupancyPercent=35 -XX:ConcGCThread=20
-XX:+PrintFlagsFinal -XX:+PrintReferenceGC -verbose:gc -XX:+PrintGCDetails -XX:+PrintGCTimeStamps -XX:+PrintAdaptiveSizePolicy
```

이 정도 포스팅을 올릴 수 있을 정도로 학습을 해야 하는데, 고민이다. <http://logonjava.blogspot.kr/2015/08/java-g1-gc-full-gc.html>

G1GC를 지금 적용할 필요는 없는데, 나중에 해본다면 아래 문서를 참고해서 적용하면 좋을 것 같다. 잘 쓰여짐
<http://imp51.tistory.com/entry/G1-GC-Garbage-First-Garbage-Collector-Tuning>

## GC log 중 -XX:+PrintTenuringDistribution

```
Desired survivor size 75497472 bytes, new threshold 15 (max 15)
- age   1:   19321624 bytes,   19321624 total
- age   2:      79376 bytes,   19401000 total
- age   3:    2904256 bytes,   22305256 total
```

라인별 설명

- Desired survivor size: "To" survivor 공간 사용률로서 75Mb, old generation으로 이동하기 전에 객체가 young 에 머무는 GC의 수는 15, max 15
- age부터는 살아남은 것으로 세대 수를 나타낸다. total은 To survivor 공간에 22Mb가 남아있다는 것이다.
- old로 갈 것이 없으니 안전한 상태로 볼 수 있다.