ConstraintLayout 使用总结
=====================



#### 参考文章

- [ConstraintLayout 属性详解 和Chain的使用](https://blog.csdn.net/zxt0601/article/details/72683379)

### ConstraintLayout 组件比例

- 两个组件相互依赖、设置`layout_constraintHorizontal_weight`比例，`layout_width`设置为0dp
```xml
 <TextView
            android:id="@+id/tv_detail"
            style="@style/text_black33_14"
            android:layout_width="0dp"
            android:layout_height="45dp"
            android:layout_marginStart="15dp"
            android:layout_marginTop="30dp"
            android:background="@drawable/shape_black_stroke_corner"
            android:gravity="center"
            android:text="查看订单"
            app:layout_constraintEnd_toStartOf="@id/tv_continue" 
            app:layout_constraintHorizontal_weight="1"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toBottomOf="@id/tv_result" />

        <TextView
            android:id="@+id/tv_continue"
            style="@style/text_white_14"
            android:layout_width="0dp"
            android:layout_height="45dp"
            android:layout_marginStart="15dp"
            android:layout_marginTop="30dp"
            android:layout_marginEnd="15dp"
            android:background="@drawable/shape_black_corner"
            android:gravity="center"
            android:text="继续逛逛"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintHorizontal_weight="1"
            app:layout_constraintStart_toEndOf="@id/tv_detail"
            app:layout_constraintTop_toBottomOf="@id/tv_result" />
```

