在 MyBatis 中，参数类型的使用与注意事项取决于 SQL 语句的类型和参数的来源。以下是常见的参数类型及其在 MyBatis 中的具体使用和注意事项：

1. **基本类型参数**：
   - 使用基本类型参数时，直接在 SQL 语句中使用 `${}` 或 `#{}` 进行引用。
   - 注意事项：确保参数类型与 SQL 语句中的占位符类型匹配，避免 SQL 注入风险。
```java
   public interface UserMapper {
       User selectUserById(Integer id);
   }
   
   ```

```xml
<select id="getUserById" resultType="User">
    SELECT * FROM users WHERE id = #{userId}
</select>
```

2. **JavaBean 类型参数**：
   - 将 JavaBean 作为参数传递给 SQL 语句，使用 `#{}` 引用 JavaBean 中的属性。
   - 注意事项：JavaBean 中的属性名称必须与 SQL 语句中的参数名称匹配。

```xml
<select id="getUserByName" resultType="User">
    SELECT * FROM users WHERE username = #{username}
</select>
```

3. **Map 类型参数**：
   - 可以将 Map 作为参数传递给 SQL 语句，使用 `#{key}` 引用 Map 中的键值对。
   - 注意事项：确保 Map 中包含 SQL 语句中所需的键值对。

```xml
<select id="getUserByMap" resultType="User">
    SELECT * FROM users WHERE id = #{id} AND username = #{username}
</select>
```

1. **数组或集合类型参数**：
   - 可以将数组或集合作为参数传递给 SQL 语句，在动态 SQL 中使用 foreach 进行遍历。
   - 注意事项：在使用 foreach 时，确保集合不为空，否则可能导致 SQL 语法错误。

```xml
<select id="getUsersByIds" resultType="User">
    SELECT * FROM users WHERE id IN 
    <foreach item="id" collection="ids" open="(" separator="," close=")">
        #{id}
    </foreach>
</select>
```

5. **多个参数**：
   - 当 SQL 语句需要多个参数时，可以使用 `@Param` 注解为参数指定名称，然后在 SQL 语句中引用。
   - 注意事项：确保 `@Param` 注解指定的参数名称与 SQL 语句中的参数名称匹配。

```java
public User getUserByIdAndName(@Param("id") int id, @Param("username") String username);
```

```xml
<select id="getUserByIdAndName" resultType="User">
    SELECT * FROM users WHERE id = #{id} AND username = #{username}
</select>
```

6. **动态 SQL 参数**：
   - 当 SQL 语句的条件需要根据参数的值动态生成时，可以使用动态 SQL。
   - 注意事项：确保使用合适的条件语句，避免因参数为空而导致的 SQL 语法错误。

```xml
<select id="getUserByCondition" resultType="User">
    SELECT * FROM users
    <where>
        <if test="id != null">
            AND id = #{id}
        </if>
        <if test="username != null">
            AND username = #{username}
        </if>
    </where>
</select>
```

通过合理选择参数类型并注意参数的传递方式和 SQL 语句的编写，可以有效地利用 MyBatis 来管理和执行 SQL 语句。