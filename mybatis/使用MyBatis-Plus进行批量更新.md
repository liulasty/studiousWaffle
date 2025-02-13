假设我们有多个用户 ID，要更新这些用户的邮箱地址：
```java
import com.baomidou.mybatisplus.core.conditions.Wrapper;
import com.baomidou.mybatisplus.core.conditions.update.UpdateWrapper;
import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import org.apache.ibatis.annotations.Param;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class UserService {
    private final UserMapper userMapper;

    public UserService(UserMapper userMapper) {
        this.userMapper = userMapper;
    }

    public void batchUpdateEmail(List<Integer> userIds, String newEmail) {
        // 创建User实体对象，只设置需要更新的字段
        User user = new User();
        user.setEmail(newEmail); // 假设email是User类中的一个字段

        // 创建UpdateWrapper来指定更新条件
        UpdateWrapper<User> updateWrapper = new UpdateWrapper<>();
        updateWrapper.in("user_id", userIds); // 使用in条件，传入一个用户ID的集合

        // 调用update方法进行批量更新
        int rowsAffected = userMapper.update(user, updateWrapper);

        if (rowsAffected > 0) {
            System.out.println("批量更新用户邮箱成功，影响了 " + rowsAffected + " 行！");
        } else {
            System.out.println("未找到符合条件的用户进行更新。");
        }
    }
}

```
在这个示例中，我们创建了一个 UpdateWrapper，使用 in 方法指定 user_id 字段应该在给定的 userIds 集合内。然后，调用 userMapper. Update 方法，传入更新后的用户对象和更新条件，来批量更新满足条件的所有用户邮箱。
注意，这个示例假设 userMapper 接口中已经包含了 update 方法，