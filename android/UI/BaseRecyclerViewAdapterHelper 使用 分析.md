BaseRecyclerViewAdapterHelper 使用 分析
====================

### 多布局

- model 实现MultiItemEntity，重要的是`getSpanSize` 和 `getItemType`这两个方法。

```java
public class MultipleItem implements MultiItemEntity {
    public static final int TEXT = 1;
    public static final int IMG = 2;
    public static final int IMG_TEXT = 3;
    public static final int TEXT_SPAN_SIZE = 3; // 这个是4列的宫格，它要占3个宫格位置
    public static final int IMG_SPAN_SIZE = 1; // 这个是4列的宫格，它要占1个宫格位置
    public static final int IMG_TEXT_SPAN_SIZE = 4; // 这个是4列的宫格，它要占4个宫格位置
    public static final int IMG_TEXT_SPAN_SIZE_MIN = 2; // 这个是4列的宫格，它要占2个宫格位置
    private int itemType;
    private int spanSize;

    public MultipleItem(int itemType, int spanSize, String content) {
        this.itemType = itemType;
        this.spanSize = spanSize;
        this.content = content;
    }

    public MultipleItem(int itemType, int spanSize) {
        this.itemType = itemType;
        this.spanSize = spanSize;
    }

    public int getSpanSize() {
        // 返回占几个宫格
        return spanSize;
    }

    public void setSpanSize(int spanSize) {
        this.spanSize = spanSize;
    }

    private String content;

    public String getContent() {
        return content;
    }

    public void setContent(String content) {
        this.content = content;
    }

    @Override
    public int getItemType() {
        return itemType;
    }
}
```
- 继承`BaseMultiItemQuickAdapter` 

```java
public class MultipleItemQuickAdapter extends BaseMultiItemQuickAdapter<MultipleItem, BaseViewHolder> {

    public MultipleItemQuickAdapter(Context context, List data) {
        super(data);
        // 添加视图的类型
        addItemType(MultipleItem.TEXT, R.layout.item_text_view);
        addItemType(MultipleItem.IMG, R.layout.item_image_view);
        addItemType(MultipleItem.IMG_TEXT, R.layout.item_img_text_view);
    }

    @Override
    protected void convert(BaseViewHolder helper, MultipleItem item) {
        // 根据不同的类型，设置不同的视图
        switch (helper.getItemViewType()) {
            case MultipleItem.TEXT:
                helper.setText(R.id.tv, item.getContent());
                break;
            case MultipleItem.IMG_TEXT:
                switch (helper.getLayoutPosition() %
                        2) {
                    case 0:
                        helper.setImageResource(R.id.iv, R.mipmap.animation_img1);
                        break;
                    case 1:
                        helper.setImageResource(R.id.iv, R.mipmap.animation_img2);
                        break;
                }
                break;
        }
    }
}
```

- 设置`LayoutManager`为`GridLayoutManager`,重要的是`MultipleItemQuickAdapter`设置`setSpanSizeLookup`,它会返回不同数据类型的占位情况。

```java
 mRecyclerView = (RecyclerView) findViewById(R.id.rv_list);
        final List<MultipleItem> data = DataServer.getMultipleItemData();
        final MultipleItemQuickAdapter multipleItemAdapter = new MultipleItemQuickAdapter(this, data);
        final GridLayoutManager manager = new GridLayoutManager(this, 4);
        mRecyclerView.setLayoutManager(manager);
        multipleItemAdapter.setSpanSizeLookup(new BaseQuickAdapter.SpanSizeLookup() {
            @Override
            public int getSpanSize(GridLayoutManager gridLayoutManager, int position) {
                // 不同类型的数据占位情况
                return data.get(position).getSpanSize();
            }
        });
        mRecyclerView.setAdapter(multipleItemAdapter);
```


