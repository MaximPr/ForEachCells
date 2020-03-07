# Обход всех клеток на прямой между двумя точками

Часто используют альгоритм Брезенхейма или провряют все точки на прямой с мелким шагом. Оба подхода выдают ошибочные ответы на некоторых примерах. Этот пример кода дает точное решение задачи. Ищу как называется этот алгоритм. Если кто-то узнает его - отпиштиесь в issues

В случае целых координат решение выглядит так:

```c#
    public static void ForEachCells(int x1, int y1, int x2, int y2, Action<int, int> f)
    {
        f(x1, y1);
        int absDx = Math.Abs(x2 - x1);
        int absDy = Math.Abs(y2 - y1);
        if (absDx == 0 && absDy == 0)
            return;

        int x = x1, y = y1;
        int nextDiffX = 1;
        int nextDiffY = 1;
        int dx = Math.Sign(x2 - x1);
        int dy = Math.Sign(y2 - y1);

        while (x != x2 || y != y2)
        {
            int profitY = nextDiffY * absDx;
            int profitX = nextDiffX * absDy;
            if (profitY >= profitX)
            {
                x += dx;
                nextDiffX += 2;
            }

            if (profitY <= profitX)
            {
                y += dy;
                nextDiffY += 2;
            }

            f(x, y);
        }
    }
```

```c#
    ForEachCells(0, 0, 3, 7, (x, y) => Console.WriteLine($"({x},{y})"));
```

```
output:
(0,0)
(0,1)
(1,1)
(1,2)
(1,3)
(2,4)
(2,5)
(2,6)
(3,6)
(3,7)
```

Альгоритм легко обобщается на случай дробных координат начала и конца отрезка
