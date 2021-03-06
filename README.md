# RainbowMVP
Lightweight MVP library(for personal using) with easy lifecycle.

For good understanding approach read this [article](https://medium.com/@czyrux/presenter-surviving-orientation-changes-with-loaders-6da6d86ffbbf).

* Really lighweight library
* Easy integrate to project
* Minimum actions to implement MVP
* Doesn't destroy presenter after rotation device
* You can cached data in presenter for restore data after rotate device

In presenter you have 3 methods:
- bindView(V view) - you need call this method for attach your view to presenter.
- unbindView() - you need call this method when view not available already.
- onDestoy() - call, when your presenter will destroy.

Important thing:
You can bind your view to presenter <b>ONLY</b> after <b>onStart()</b>

[![](https://jitpack.io/v/ne1c/rainbowmvp.svg)](https://jitpack.io/#ne1c/rainbowmvp)
[![Android Arsenal](https://img.shields.io/badge/Android%20Arsenal-RainbowMVP-green.svg?style=true)](https://android-arsenal.com/details/1/4112)

#Dependency

Step 1. Add it in your root build.gradle at the end of repositories:
```groovy
allprojects {
	repositories {
		...
		maven { url "https://jitpack.io" }
	}
}
```

Step 2. Add the dependency
```groovy
dependencies {
	   compile 'com.github.ne1c:rainbowmvp:1.0'
	}
}
```

# Example how to integrate library in your project:

# Step 1
Create you View interface:

```java
public interface MyView {
    void showResult(String text);
    
    void showError(@StringRes int resId);
}
```

# Step 2
Create presenter. You need inherit of BasePresenter and add tag:

```java
public class MyPresenter extends BasePresenter<MyView> {
    public static final TAG = MyPresenter.class.getName();
    ...
    public void makeParty() {
        // some actions
        boolean success = ...
        if (success) {
            mView.showResult(...);
        } else {
            mView.showError(R.string.error);
        }
    }
    ...
}
```
# Step 3
* Implement PresenterStorage:
```java
public class MyPresenterStorage implements PresenterStorage {
    public MyPresenterStorage(...) {
        ...
    }

    @Override
    public BasePresenter create(String tag) {
        if (tag.equals(MyPresenter.TAG)) {
            return new MyPresenter(...);
        }

        return null;
    }
}
```

# Step 4
* Init PresenterFactory, before use any presenter. You can do it in onCreate() of Applicaton or splash screen:
```java
public class MyApplication extends android.app.Application {
    @Override
    public void onCreate() {
        ...
        PresenterFactory.init(new MyPresenterStorage(...));
    }
}
```

# Step 5
Your activity or fragment need to inherit of BaseActivity/BaseFragment and override getPresenterTag():
```java
public class MyActivity extends BaseActivity<MyPresenter> implements MyView {
    ...
    @Ovveride
    public void onStart() {
        super.onStart();
        
        mPresenter.bindView(this);
    }
    
    @Ovveride
    public void onStop() {
        super.onStop();
        
        mPresenter.unbindView();
    }
    
    @Override
    public String getPresenterTag() {
        return MyPresenter.TAG;
    }
    ...
}
```

<h2>
    <a id="user-content-license" class="anchor" href="#license" aria-hidden="true">
    <span class="octicon octicon-link"></span></a>License
</h2>
<pre><code>Copyright 2016 Nikolay Kucheriaviy

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
</code></pre>
