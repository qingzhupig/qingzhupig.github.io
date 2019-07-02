---
layout: post
title: 回溯和N皇后问题
date: 2017-03-12 16:21:10 +0800
categories: 
published: false
---

回溯算法一直是我比较头疼的问题，头疼是因为没怎么学过，没学是因为觉得这个算法太不优雅了，有点排斥:-)

哈哈，下午无聊，稍微看一下[N皇后问题](https://segmentfault.com/a/1190000003733325)。

代码如下：

``` java
import java.util.Arrays;
import java.util.function.Consumer;

public class Queen {
    private static int EMPTY = -1;

    private final int[] queens;

    public Queen(int n) {
        if (n < 1) throw new IllegalArgumentException("positive queen count required");

        this.queens = new int[n];
        for (int i = 0; i < n; ++n) {
            queens[i] = EMPTY;
        }
    }

    public void findAvailableSets(Consumer<int[]> onFind) {
        put(0, onFind);
    }

    private void put(int row, Consumer<int[]> onSetup) {
        if (row == queens.length) {
            onSetup.accept(queens);
        }
        else {
            for (int ci = 0; ci < queens.length; ++ci) {
                if (isSafe(row, ci)) {
                    queens[row] = ci;
                    put(row + 1, onSetup);
                    queens[row] = EMPTY;
                }
            }
        }
    }

    private boolean isSafe(int row, int col) {
        // check based on col equality
        for (int ri = 0; ri < row; ++ri) {
            if (queens[ri] == col) {
                return false;
            }
        }

        // check on diagonal line
        for (int ri = 0; ri < row; ++ri) {
            int c = queens[ri];
            if (c != -1 && Math.abs(queens[ri] - col) == Math.abs(ri - row)) {
                return false;
            }
        }

        return true;
    }

    public static void main(String[] args) {
        int N = 4;
        new Queen(N).findAvailableSets(queens -> System.out.println(Arrays.toString(queens)));
    }
}
```

执行结果：

```
[1, 3, 0, 2]
[2, 0, 3, 1]
```

