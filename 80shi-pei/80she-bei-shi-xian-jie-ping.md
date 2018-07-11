#### 8.0设备实现截屏

在8.0+设备上，强制使用了硬件绘制，并且原先软件绘制已经被遗弃。

> buildDrawingCache\(boolean\)在8.0设备上已经被遗弃了，调用会直接崩溃，并报错：Software rendering doesn't support hardware bitmaps

此时采用获取原先绘制缓存的方式来进行截屏已经无解！

另一个方案是，自己创建一张Bitmap，交给Canvas，自行绘制：

```
/**
 * Created by HSH on 18-7-10.
 * 将该布局套在需要进行截屏的View上即可。
 */
public class SofeWareFrameLayout extends FrameLayout {

    private Bitmap bitmap;

    private Canvas canvas;

    public SofeWareFrameLayout(@NonNull Context context) {
        super(context);
        setWillNotDraw(false);
    }

    public SofeWareFrameLayout(@NonNull Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
        setWillNotDraw(false);
    }

    public SofeWareFrameLayout(@NonNull Context context, @Nullable AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        setWillNotDraw(false);
    }

    public Bitmap getBitmap() {
        // 将控件的内容绘制到Canvas上
        draw(canvas);
        // 使用完Bitmap必须主动进行回收
        return bitmap;
    }

    @Override
    protected void onSizeChanged(int w, int h, int oldw, int oldh) {
        super.onSizeChanged(w, h, oldw, oldh);
        bitmap = Bitmap.createBitmap(w, h,
                Bitmap.Config.ARGB_8888);
        canvas = new Canvas(bitmap);
    }
}
```



