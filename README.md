# Transition
转场动画(自定义切换动画) 
1.（转场协议Transition <UIViewControllerTransitioningDelegate>）
转场协议中可以确定AnimationTranstioning ，UIPresentationController。

2.（转场动画实现 AnimationTranstioning  NSObject<UIViewControllerAnimatedTransitioning>）
转场动画中实现两个方法
- (NSTimeInterval)transitionDuration:(id<UIViewControllerContextTransitioning>)transitionContext
{
    return duration;//动画时间
}
- (void)animateTransition:(id<UIViewControllerContextTransitioning>)transitionContext
{
动画的内容
}

3. 写一个类继承于UIPresentationController
UIPresentationController 的子类是负责「被呈现」(presented) 及「负责呈现」(presenting) 的 controller 以外的 controller 的
必须实现4个方法
- (void)presentationTransitionWillBegin;////在呈现过渡即将开始的时候被调用的

- (void)presentationTransitionDidEnd:(BOOL)completed;////在呈现过渡结束时被调用的

- (void)dismissalTransitionWillBegin;//在退出过渡即将开始的时候被调用的

- (void)dismissalTransitionDidEnd:(BOOL)completed;//在退出的过渡结束时被调用的


首先 如果是通过navigationcontroller来转场的，让根控制器遵守<UINavigationControllerDelegate>
-(void)viewDidAppear:(BOOL)animated{
    self.navigationController.delegate = self;
}
然后实现代理方法
- (id <UIViewControllerAnimatedTransitioning>)navigationController:(UINavigationController *)navigationController
                                   animationControllerForOperation:(UINavigationControllerOperation)operation
                                                fromViewController:(UIViewController *)fromVC
                                                  toViewController:(UIViewController *)toVC{
    if (operation == UINavigationControllerOperationPush) {

        返回一个自定义转场动画 在自定转场动画中写转场效果
       AnimationTranstioning *ping = [AnimationTranstioning new];
        return ping;
    }else{
        return nil;
    }
}

然后 如果是两个控制器之间的present有两个方法来确定转场协议
1.在第一个控制器中在创建第二个控制器时
//转场效果为自定义
 prensentV.modalTransitionStyle = UIModalPresentationCustom;
//遵守转场协议（协议中确定转场动画和UIPresentationController）
    prensentV.transitioningDelegate = [GYTransition sharedtransition];
//present
        [self presentViewController:prensentV animated:YES completion:nil];
2. 在ViewController中遵守UIViewControllerTransitioningDelegate
实现代理方法 返回自定义的转场动画
- (id <UIViewControllerAnimatedTransitioning>)animationControllerForPresentedController:(UIViewController *)presented presentingController:(UIViewController *)presenting sourceController:(UIViewController *)source{
    return self.presentAnimation;

}
