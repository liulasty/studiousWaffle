###### 无状态（STATELESS）会话管理策略，这意味着不依赖 HttpSession 进行用户状态管理。这与前面的基于用户名/密码的登录认证及设置认证上下文操作相吻合。在无状态模式下，认证信息通常以 JWT 等形式附在请求头中传递，而不是保存在服务器端的 HttpSession 中。在后续每次非白名单请求时，需要使用 jwt 携带信息进行设置认证上下文吗？
在无状态（STATELESS）会话管理策略下，确实不需要依赖 HttpSession 来管理用户状态。当您使用基于用户名/密码的登录认证并设置认证上下文后，服务器会生成一个 JWT（JSON Web Token）并将它返回给客户端。客户端应在后续请求中将这个 JWT 作为 Authorization 头的一部分附在每个非白名单请求中发送给服务器。
服务器端的 jwtAuthorizationFilter（或其他处理 JWT 的过滤器或拦截器）负责从请求头中提取 JWT，验证其有效性，并从中提取用户信息。一旦验证通过，应重新设置认证上下文，如下所示：


```
UsernamePasswordAuthenticationToken authenticationToken = new UsernamePasswordAuthenticationToken (
    Principal, // 从 JWT 中提取的用户主体信息，如用户名、角色等
    Null, // 密码为空，因为在无状态模式下，密码无需在后续请求中再次验证
    Authorities // 从 JWT 中提取的权限信息
);

SecurityContextHolder.GetContext (). SetAuthentication (authenticationToken);
```

这样，每次非白名单请求到达服务器时，jwtAuthorizationFilter 会自动处理 JWT，验证用户身份，并重新设置认证上下文。这样，后续的安全决策（如访问控制、权限检查等）就可以基于当前请求的认证上下文来进行，而无需依赖 HttpSession。
总结一下，在无状态会话管理策略下，对于非白名单请求：
- 客户端应将 JWT 附在请求头中发送给服务器。
- 服务器端的 JWT 过滤器验证 JWT 的有效性，并从 JWT 中提取用户信息。
- 过滤器使用提取的信息创建一个新的 UsernamePasswordAuthenticationToken（或其他合适的认证令牌），并将其设置到 SecurityContextHolder 中。
- 服务器根据设置的认证上下文进行后续的安全决策和访问控制。
通过这种方式，即使不依赖 HttpSession，也能确保每个请求都有与其关联的认证上下文，从而实现无状态会话管理下的用户身份验证和授权。

`