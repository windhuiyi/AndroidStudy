SwipeRefreshLayout 汇总
=====================

### SwipeRefreshLayout 子视图

- SwipeRefreshLayout的子视图必须是ListView，RecyclerView，ScrollView，GridView等可滑动的控件。而且只能有一个控件。

### SwipeRefreshLayout 没有滑动到顶部就可以刷新

- 因为SwipeRefreshLayout和子视图RecyclerView的滑动冲突导致。RecyclerView添加滑动监听，当RecyclerView滑动到顶部的时候，SwipeRefreshLayout才可以刷新。

```java
bindingView.get().rvList.addOnScrollListener(new RecyclerView.OnScrollListener(){
            @Override
            public void onScrolled(RecyclerView recyclerView, int dx, int dy) {
                int topRowVerticalPosition = (recyclerView == null || recyclerView.getChildCount() == 0) ? 0 : recyclerView.getChildAt(0).getTop();
                bindingView.get().srlList.setEnabled(topRowVerticalPosition >= 0);

            }
            @Override
            public void onScrollStateChanged(RecyclerView recyclerView, int newState) {
                super.onScrollStateChanged(recyclerView, newState);
            }
        });
```