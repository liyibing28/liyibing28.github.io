---
layout: post
title: warmshowers利用account实现账户管理
category: 技术
tags: warmshwoers android 学习
keywords: 
description: 
---

android学习～～利用google accountmanager实现账户管理

为什么要学习accountmanager ，warmshowers android app中选择account进行管理。
好处：标准的用户鉴权方式；为开发者简化了登录的流程；处理访问拒绝的场景；可以为一个账户处理不同类型的访问令牌（如：只读、全权限）；轻松的在不同程序间共享令牌；有如Sync Adapter这样的后台处理的良好支持；并且，在手机的设置界面中有一个很酷的入口

### 参考资料： 
http://blog.csdn.net/wy3243996/article/details/52411139

* Authentication Token：是由服务器提供的一个临时的访问令牌。所有需要识别用户的请求，在发送到服务器时都要带着这个令牌。

* AccountManager– 管理设备上的所有账户，也是这项功能的核心。App从 AccountManager 获得auth-token，它也将决定是否该打开登录、创建用户的 Activity ，或者从之前的请求中返回一个已经存储好的auth-token。 AccountManager 了解不同场景下该调用何种操作。

- AccountAuthenticator– 是一个为具体账户类型提供鉴权处理过程的组件。 AccountManager 查找合适的 AccountAuthenticator ，与其通信，并根据账户类型执行所有动作。 AccountAuthenticator 知道哪个 Activity 用来让用户输入登录信息，也知道服务器上次返回的auth-token在哪里存储。在一个账户类型下，多个不同的服务也会共用同一个 AccountAuthenticator 。比如Google的AccountAuthenticator 为GMail提供认证服务，也为其他的Google程序，如：Google Calendar和Google Drive提供授权服务。

- AccountAuthenticatorActivity– “登录／创建用户“ Activity 的基类，当用户需要认证的时候， authenticato r调用会这个 Activity 。这个 Activity 负责用户登录或用户创建过程，并将auth-token返回给

authenticator


当你的App需要auth-token时，只需调用 AccountManager#getAuthToken() 。 AccountManager 将负责一切必须的步骤直到给你拿到auth-token。Google提供了一个流程图展现了整个过程：

- mAuthTokenType 是我从服务器请求的令牌的类型

### warmshower结构：
- AuthenticatorActivity是唯一与用户交互的界面，继承自WSSupportAccountAuthenticatorActivity。主要进行与用户登录，注册，注销，更新页面的交互。 
 - ``` java
  public void applyCredentials(View view){
       String username = editUsername.getText().toString();
       String password = editPassword.getText().toString();
       if (!username.isEmpty() && !password.isEmpty()) {
           //mDialogHandler.showDialog(DialogHandler.AUTHENTICATE);
           Account account = AuthenticationHelper.createNewAccount(username, password);
           AuthenticationTask authTask = new AuthenticationTask();
           authTask.execute();
       }
   }
  ```
  ```Account account = AuthenticationHelper.createNewAccount(username, password);```
  直接讲获取到的用户名密码传入createNewAccount函数，交给accountmanager进行管理，不用验证账户正确性。之后调用
  ```
  AuthenticationTask authTask = new AuthenticationTask();
           authTask.execute();
  ```
```java
​``` java
public class AuthenticationTask extends AsyncTask<Void, Void, Void> {
        int mUID = 0;
        boolean mNetworkError = false;

        protected Void doInBackground(Void... params) {

            HttpAuthenticator authenticator = new HttpAuthenticator();
            try {
                mUID = authenticator.authenticate();
            } catch (IOException e) {
                mNetworkError = true;
            } catch (JSONException e) {
                mNetworkError = true;
            } catch (Exception e) {
                // mUid value will capture anything else.
            }
            return null;
        }
        protected void onPostExecute(Void v) {
            //mDialogHandler.dismiss();

            if (mUID < 1) {
                if (mNetworkError) {
                    Toast.makeText(AuthenticatorActivity.this, R.string.http_server_access_failure, Toast.LENGTH_SHORT).show();
                } else {
                    Toast.makeText(AuthenticatorActivity.this, R.string.authentication_failed, Toast.LENGTH_LONG).show();
                }
                AuthenticationHelper.removeOldAccount();
                // And just stay on the page auth screen.
            }
            // Otherwise launch the maps activity, with no history
            else {
                Intent intent = new Intent(AuthenticatorActivity.this, Map2Activity.class);
                intent.addFlags(Intent.FLAG_ACTIVITY_NO_HISTORY);
                startActivity(intent);
            }
        }
    }
```
- AuthenticationHelper 用于account管理，主要实现 在account中创建一个新账户 从accoutn中获取到warmshowers账户 获取账户cookie 获取账户uid 获取账户用户名 获取token（代替password）进行验证 删除账户 增加cookie
 - 创建新账户.
``` java
    public static Account createNewAccount(String username, String password) {
        AccountManager accountManager = AccountManager.get(WSAndroidApplication.getAppContext());
        Account account = new Account(username, AuthenticationService.ACCOUNT_TYPE);
        accountManager.addAccountExplicitly(account, null, null);
        accountManager.setAuthToken(account, AuthenticationService.ACCOUNT_TYPE, password);
        return account;
    }
```
 - 在accountmanager中获取warmshowers账户.
``` java
   		public static Account getWarmshowersAccount() throws NoAccountException {
        AccountManager accountManager = AccountManager.get(WSAndroidApplication.getAppContext());
        Account[] accounts = accountManager.getAccountsByType(AuthenticationService.ACCOUNT_TYPE);
        if (accounts.length == 0) {
            throw new NoAccountException();
        }

        return accounts[0];
    }
```
 - 