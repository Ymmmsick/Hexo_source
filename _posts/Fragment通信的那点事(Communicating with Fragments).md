title: Fragment通信的那点事(Communicating with Fragments)
date: 2016-03-09 17:01:47
categories: Android
tags: Fragments通信,Fragment communicating
---

Fragment通信情况分为两种：

1.处于同一Activity中的Fragment通信，经典的例如Activity+ViewPager+Fragment

2.处于非同一Activity中的两个Fragment或一个Activity中的Fragment与另外的Activity通信

因为第二种较为简单，这里先说一说第二种个人解决方案，第二种直接采用receiver广播机制来通信（发送参数，更新界面等）

遇到第一种情况虽然也可以使用receiver广播来解决，但是广播机制肯定会涉及到线程开销的，所以从性能着想，采用google官方推荐的接口通信。

<b>定义一个接口</b>

为了让一个Fragment传达到它的Activity，可以在Fragment内部定义一个接口让Activity来实现。该Fragment的onAttach（）方法的生命周期过程中捕获接口实现，然后就可以调用接口的方法，以便与Activity进行沟通。

这里是Fragment与Activity通信的例子：

	public class HeadlinesFragment extends ListFragment {
    OnHeadlineSelectedListener mCallback;

    // Container Activity must implement this interface
    public interface OnHeadlineSelectedListener {
        public void onArticleSelected(int position);
    }

    @Override
    public void onAttach(Activity activity) {
        super.onAttach(activity);
        
        // This makes sure that the container activity has implemented
        // the callback interface. If not, it throws an exception
        try {
            mCallback = (OnHeadlineSelectedListener) activity;
        	} catch (ClassCastException e) {
            	throw new ClassCastException(activity.toString()
                    + " must implement OnHeadlineSelectedListener");
        	}
    	}
    
    ...
	}
从代码中可以看出Fragment捕获了Activity的接口实现，这是第一个Fragment已经完成了该有的通信操作，接下来应该由Activity将接受到的数据传递给另外一个Fragment了

	public static class MainActivity extends Activity
        implements HeadlinesFragment.OnHeadlineSelectedListener{
    ...

    public void onArticleSelected(int position) {
        // The user selected the headline of an article from the HeadlinesFragment
        // Do something here to display that article

        ArticleFragment articleFrag = (ArticleFragment)
                getSupportFragmentManager().findFragmentById(R.id.article_fragment);

        if (articleFrag != null) {
            // If article frag is available, we're in two-pane layout...

            // Call a method in the ArticleFragment to update its content
            articleFrag.updateArticleView(position);
        } else {
            // Otherwise, we're in the one-pane layout and must swap frags...

            // Create fragment and give it an argument for the selected article
            ArticleFragment newFragment = new ArticleFragment();
            Bundle args = new Bundle();
            args.putInt(ArticleFragment.ARG_POSITION, position);
            newFragment.setArguments(args);
        
            FragmentTransaction transaction = getSupportFragmentManager().beginTransaction();

            // Replace whatever is in the fragment_container view with this fragment,
            // and add the transaction to the back stack so the user can navigate back
            transaction.replace(R.id.fragment_container, newFragment);
            transaction.addToBackStack(null);

            // Commit the transaction
            transaction.commit();
        	}
    	}
	}

通过上面的例子，展示了Fragment与Fragment之间用接口来实现通信的功能，以后不能滥用广播了~~~