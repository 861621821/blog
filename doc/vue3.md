- #### Fragment

  > Vue3 中可以有多个根节点

- #### diff

  > vue2.x 中虚拟 dom 是全量比较

  > vue3.0 采用静态标记（PatchFlag）:`createVNode('p', null, 'Vue3', 1 /* TEXT */)`，只有存在静态标记的虚拟 dom 才会比较
