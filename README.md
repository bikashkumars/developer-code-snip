# developer-code-snip
Easy Coding Tips and Tricks



## Spring Boot and JPA

### Resopistory

- PagingAndSortingRepository
- JpaSpecificationExecutor

```java
import java.util.List;
import java.util.Set;
public interface ExampleDAO extends PagingAndSortingRepository<UserEntity, String>, JpaSpecificationExecutor<UserEntity> {
    @Query(value = "SELECT be FROM UserEntity be " +
            "WHERE be.status = 1 OR be.status = 2 OR be.status = 3 " +
            "AND zoneName = ?1 ")
    List<UserEntity> getExecutableBatchJobs(String zoneName);
    List<UserEntity> findByStatusAndZoneName(int status, String zoneName);
    Page<UserEntity> findByStatus(int status, Pageable pageable);
    Page<UserEntity> findByTenant(String tenant, Pageable pageable);
    Page<UserEntity> findByTenantAndStatus(String tenant, int status, Pageable pageable);
    @Query(value = "SELECT be FROM UserEntity be " +
            "WHERE id IN ( " +
            "SELECT period.batch.id FROM BatchSchedulePeriod period " +
            "WHERE period.status != 2 " +
            ") " +
            "AND zoneName = ?1")
    List<UserEntity> getBatchJobsWhichHaveUnFinishPeriods(String zoneName);
    @Query(value = "SELECT * FROM device_management.scef_batch be where be.action = ?1", nativeQuery = true)
    List<UserEntity> findByActionName(String actionName);
    UserEntity findByIdAndDeletedFalse(String id);
    List<UserEntity> findByIdIn(Set<String> ids);
    List<UserEntity> findByIdInAndDeletedFalse(Set<String> ids);
    List<UserEntity> findByIdInAndTenantAndDeletedFalse(Set<String> ids, String tenant);
    List<UserEntity> findByStatusAndZoneNameAndDeletedFalseAndCancelFalse(int status, String zoneName, Pageable pageable);
    List<UserEntity> findByStatusInAndZoneNameAndDeletedFalse(int[] status, String zoneName, Pageable pageable);
    @Modifying
    @Transactional
    @Query(value = "update UserEntity be set be.currentItemSequence = be.currentItemSequence+?2 where id = ?1")
    void increaseCurrentItemSequence(String id, int increment);
}
```
