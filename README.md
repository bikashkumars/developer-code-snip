# developer-code-snip
Easy Coding Tips and Tricks



## Spring Boot and JPA

### Resopistory

- PagingAndSortingRepository
- JpaSpecificationExecutor
- JpaRepository

JpaRepository contains the full API of CrudRepository and PagingAndSortingRepository.

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


### Transction Example

```java
import java.util.List;
import java.util.Set;
public interface UserDAO extends PagingAndSortingRepository<User, String>, JpaSpecificationExecutor<User> {
    User findByDeviceId(String item);
    User findByJobId(String jobId);
    List<User> findByBatchIdAndStatus(String batchId, int status);
    List<User> findByBatchId(String batchId);
    Page<User> findByBatchId(String batchId, Pageable pageable);
    Page<User> findByBatchIdAndStatus(String batchId, int status, Pageable pageable);
    Page<User> findByBatchIdAndStatusIn(String batchId, List<Integer> statuses, Pageable pageable);
    Page<User> findByStatus(int status, Pageable pageable);
    long countByBatchIdAndStatus(String batchId, int status);
    long countByBatchId(String batchId);
    
	@Modifying
    @Transactional
    void deleteByBatchId(String batchId);
	
	
    User findByBatchIdAndSequenceAndStatus(String batchJobId, int sequence, int status);
	
    @Query(value = "SELECT i FROM User i WHERE i.status = 0 OR i.status = 1 AND i.batchId = ?1")
    List<User> findUnsuccessUsersByBatchIdAndStatus(String batchId);
	
    User findByBatchIdAndDeviceId(String batchId, String deviceId);
	
    void deleteByBatchIdIn(Set<String> ids);
    
	@Modifying
    @Transactional
    @Query(value = "update User i set i.status = ?2 where i.batchId = ?1 and i.status in (0, 1, 4, 6, -1)")
    void updateUserStatusByBatchId(String batchId, int status);
    
	@Modifying
    @Transactional
    @Query(value = "update User i set i.status = ?1 where i.batchId = ?2 and i.status in ?3")
    void updateUserStatusByBatchIdAndStatus(int newStatus, String batchId, List<Integer> statuses);
    
	@Modifying
    @Transactional
    @Query(value = "update User i set i.status = ?2 where i.jobId = ?1")
    void updateUserStatusByJobId(String jobId, int status);
}
```
