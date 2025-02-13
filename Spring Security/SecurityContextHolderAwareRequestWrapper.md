```java
//  
// Source code recreated from a .class file by IntelliJ IDEA  
// (powered by FernFlower decompiler)  
//  
  
package org.springframework.security.web.servletapi;  
  
import java.security.Principal;  
import java.util.Collection;  
import java.util.Iterator;  
import javax.servlet.http.HttpServletRequest;  
import javax.servlet.http.HttpServletRequestWrapper;  
import org.springframework.security.authentication.AbstractAuthenticationToken;  
import org.springframework.security.authentication.AuthenticationTrustResolver;  
import org.springframework.security.authentication.AuthenticationTrustResolverImpl;  
import org.springframework.security.core.Authentication;  
import org.springframework.security.core.GrantedAuthority;  
import org.springframework.security.core.context.SecurityContextHolder;  
import org.springframework.security.core.userdetails.UserDetails;  
import org.springframework.util.Assert;  
  
public class SecurityContextHolderAwareRequestWrapper extends HttpServletRequestWrapper {  
    private final AuthenticationTrustResolver trustResolver;  
    private final String rolePrefix;  
  
    public SecurityContextHolderAwareRequestWrapper(HttpServletRequest request, String rolePrefix) {  
        this(request, new AuthenticationTrustResolverImpl(), rolePrefix);  
    }  
  
    public SecurityContextHolderAwareRequestWrapper(HttpServletRequest request, AuthenticationTrustResolver trustResolver, String rolePrefix) {  
        super(request);  
        Assert.notNull(trustResolver, "trustResolver cannot be null");  
        this.rolePrefix = rolePrefix;  
        this.trustResolver = trustResolver;  
    }  
  
    private Authentication getAuthentication() {  
        Authentication auth = SecurityContextHolder.getContext().getAuthentication();  
        return !this.trustResolver.isAnonymous(auth) ? auth : null;  
    }  
  
    public String getRemoteUser() {  
        Authentication auth = this.getAuthentication();  
        if (auth != null && auth.getPrincipal() != null) {  
            if (auth.getPrincipal() instanceof UserDetails) {  
                return ((UserDetails)auth.getPrincipal()).getUsername();  
            } else {  
                return auth instanceof AbstractAuthenticationToken ? auth.getName() : auth.getPrincipal().toString();  
            }  
        } else {  
            return null;  
        }  
    }  
  
    public Principal getUserPrincipal() {  
        Authentication auth = this.getAuthentication();  
        return auth != null && auth.getPrincipal() != null ? auth : null;  
    }  
  
    private boolean isGranted(String role) {  
        Authentication auth = this.getAuthentication();  
        if (this.rolePrefix != null && role != null && !role.startsWith(this.rolePrefix)) {  
            role = this.rolePrefix + role;  
        }  
  
        if (auth != null && auth.getPrincipal() != null) {  
            Collection<? extends GrantedAuthority> authorities = auth.getAuthorities();  
            if (authorities == null) {  
                return false;  
            } else {  
                Iterator var4 = authorities.iterator();  
  
                GrantedAuthority grantedAuthority;  
                do {  
                    if (!var4.hasNext()) {  
                        return false;  
                    }  
  
                    grantedAuthority = (GrantedAuthority)var4.next();  
                } while(!role.equals(grantedAuthority.getAuthority()));  
  
                return true;  
            }  
        } else {  
            return false;  
        }  
    }  
  
    public boolean isUserInRole(String role) {  
        return this.isGranted(role);  
    }  
  
    public String toString() {  
        return "SecurityContextHolderAwareRequestWrapper[ " + this.getRequest() + "]";  
    }  
}
```
该类是一个 HttpServletRequest 的包装器，用于增强请求的安全性。它通过 SecurityContextHolder 来获取当前的认证信息，并提供了一些方法来获取用户信息和权限检查。
- 构造函数：通过 HttpServletRequest 来创建一个包装器实例，并可以指定角色前缀和认证信任解析器。
- GetRemoteUser ()：获取当前认证的用户名称，如果用户已认证且非匿名，则返回 UserDetails 的用户名或 AbstractAuthenticationToken 的名称。
- GetUserPrincipal ()：获取当前认证的用户主体，如果用户已认证且非匿名，则返回 Authentication 实例。
- IsGranted (String role)：检查当前用户是否被授予指定的角色，需要考虑角色前缀，并通过 Authentication 的 Authorities 进行验证。
- IsUserInRole (String role)：检查当前用户是否拥有指定的角色，是 isGranted () 方法的别名。
- ToString ()：返回该请求包装器的字符串表示形式，包含请求本身的信息。