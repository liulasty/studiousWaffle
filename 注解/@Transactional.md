**@Transactional 是 Spring 框架中用于声明式事务管理的一个注解，它允许你将事务管理的逻辑嵌入到业务方法中，而无需显式地调用开始和结束事务的代码。当方法带有@Transactional 注解时，Spring 会自动开启一个事务，方法执行过程中如果发生异常，事务会被回滚；如果没有异常，事务会在方法结束时提交。**
```java
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

@Service
public class UserService {

    public void updateUserEmailBatch(List<Integer> userIds, String newEmail) {
        // 假设这是一个数据库操作，批量更新多个用户邮箱
        for (Integer userId : userIds) {
            User user = userRepository.findById(userId).orElseThrow(() -> new NotFoundException("User not found"));
            user.setEmail(newEmail);
            userRepository.save(user);
        }
    }

    @Transactional
    public void safeBatchUpdateEmail(List<Integer> userIds, String newEmail) {
        try {
            updateUserEmailBatch(userIds, newEmail);
        } catch (Exception e) {
            // 在这里处理异常，事务会被回滚
            throw new RuntimeException(e.getMessage(), e);
        }
    }
}

```
在上面的代码中，safeBatchUpdateEmail 方法是带@Transactional 注解的。这意味着：
- 事务开始：当调用这个方法时，Spring 会开始一个新的数据库事务。
- 执行操作：updateUserEmailBatch 方法内部的数据库操作（如保存用户）都在同一个事务中。
- 异常处理：如果在执行过程中抛出了异常，事务会自动回滚，以确保数据一致性。
- 事务提交：如果没有异常，事务会在方法结束时自动提交。
请注意，@Transactional 注解只有在 Spring 容器管理的 bean 中使用时才会生效，因此通常放在带有@Service、@Repository 或@Component 注解的类中的方法上。此外，Spring 的事务管理需要配置，通常是在 Spring 的配置文件中启用事务管理器，并指定事务传播行为和回滚规则。