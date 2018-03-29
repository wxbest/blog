# PHP红包算法

## 剩余不断变化的平均值去加减随机数做到不超过总额

```php
/*
 * 获取随机红包
 * min<k<max
 * min(n-1) <= money - k <= (n-1)max
 * k <= money-(n-1)min
 * k >= money-(n-1)max
 */
function getRedPackage($money, $num, $min, $max)
{
    $data = array();
    if ($min * $num > $money) {
        return array();
    }
    if($max*$num < $money){
        return array();
    }
    while ($num >= 1) {
        $num--;
        $kmix = max($min, $money - $num * $max);
        $kmax = min($max, $money - $num * $min);
        $kAvg = $money / ($num + 1);
        //获取最大值和最小值的距离之间的最小值
        $kDis = min($kAvg - $kmix, $kmax - $kAvg);
        //获取0到1之间的随机数与距离最小值相乘得出浮动区间，这使得浮动区间不会超出范围
        $r = ((float)(rand(1, 10000) / 10000) - 0.5) * $kDis * 2;
        $k = round($kAvg + $r);
        $money -= $k;
        $data[] = $k;
    }
    return $data;
}
```

