# red_black_tree

red black tree impl

---

一些要点:

1.
红黑树可以转换成2-3树, 只要将红色节点往其父节点提到相同的高度, 并将这两个节点融合到一起即可

2.
红黑树是难得的 插入、搜索、删除的时间复杂度都能维持在logN的平衡二叉树, 但其不是绝对平衡的

3.
红黑树的特性:
a-> 每个节点只有两种红的或黑的
b-> 根节点绝对是黑色的
c-> 不能出现两个相邻的红色节点, 依据这个特点, 我们可以推断处一些东西
d-> 从任意节点到其所有叶子节点的简单路径上经过的黑色节点的数目是相同的


红黑树在插入、删除一个节点后, 需要根据插入、删除后是否还能维持红黑树的特性决定是否进行
调整以在插入、删除过后还是一颗红黑树, 这主要通过红黑树主要通过旋转以及着色操作来调整

---

插入节点
为了实现上面的便利, 约定待插入的节点以红节点的形式被插入到红黑树中

case 1. 插入一颗空树, 此时需要对新插入的节点进行recolour()
case 2. 在黑色节点下插入一个 子节点 , 此时不需要对树进行调整操作
case 3. 待插入的节点是红色的and其父节点也是红色的 && 且它们都是右斜的 && 没有红色uncle节点(黑色uncle或者无uncle)
        此时, 需要对以该节点的父节点作为支点进行一个左旋操作 rotate-left(), 然后, 将旋转
        过后的父节点的颜色与其左节点的颜色互换, ⚠️  如果此时是有黑色uncle的, 就要将parent的左子节点放到grandpa的右子节点上,
        然后, 将parent的左子节点设置为parent即可.
case 4. 同上, 唯一的却别就是左斜的, 调整也是按照右旋调整的 rotate-right(),
        然后, 将调整过后的父节点与其右节点的颜色互换
case 5. 与case3、4 相比, 加入了一个有红色uncle节点的限制, 在这种状况下, 有两种细分的情况需要
        却别对待, 而这个也是在基本情况主体下的细化, 所以先说下主体状况
        在插入一个节点后, 此时当前节点、父节点都是红色的, 这时我们可以推断出,grandpad节点肯定
        是黑色的, 因为在我们插入之前的树是一个红黑树, 那么, 在父节点是红色的情况下, 父节点的
        父节点也就是grandpa节点肯定是黑色的, 如果此时的uncle节点也是红色的, 那么, 执行一下的
        调整策略:
        将grandpa节点的颜色下放(darkness from grandpad)给其两个子节点, 即grandpa是什么颜色, 就
        将其两个节点的颜色染成什么颜色, 前面说到有两种细化场景, 此时, (1)如果grandpa不是root节点的话,
        需要将grandpa节点染成红色的, (2)如果grandpa是root节点的话,不需要重新染色了, 因为之前推断出来
        grandpa不然是一个黑色节点,而root节点必须是黑色的, 所以不需要重染
        颜色下方的原因是, 需要保证调整过后的以grandpa为root节点的子树到其所有叶子节点经过的所有黑节点
        的数目是一致的, 为什么往下调整, 因为grandpa以上的肯定是符合黑节点数目一致这个约束的
case 6. 当当前插入节点是红色、且当前节点的父节点是红色, 并且他们之间的结构关系是右左时, 即父节点在grandpa的右边的,
        当前节点在父节点的左边时, 形成一个right-left结构, 那么, 此时需要先进行一个右旋, 右旋过后, 形成一个right-right结构
        此时,当前节点变味父节点, 父节点变为当前节点的右节点, 由于是一个right-right右右结构, 再执行一个左旋即可, 旋转过后,
        再根据情况执行着色操作
case 7. 当前结构是left-right结构, 先执行一个左旋, 然后再进行一个右旋


⚠️ : 由于红黑树也是一个平衡二叉树, 所以其在插入一个节点后, 形成的局部结构, 也如avl一样, 只有四种情况,
    一是: right-right
    二是: left-left
    三是:right-left
    四是:left-right

    那么, 其旋转操作也是一样的, 只是红黑树又加了一些约束, 需要在旋转过后再执行一个重新染色以符合性质的操作

---




