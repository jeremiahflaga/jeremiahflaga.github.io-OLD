---
layout: post
title: 'Left Horizontal Change Handler of Conductor in android'
categories: [Programming, Book]
tags: []
date: 2018-12-19 01:20:00 PM UTC
published: false
---

<!-- April 24, 2018 09:40:00 PM Philippine Time -->

martin fowler - la

copied HorizontalChangeHandler


<!--more-->



``` java

import android.animation.Animator;
import android.animation.AnimatorSet;
import android.animation.ObjectAnimator;
import android.support.annotation.NonNull;
import android.support.annotation.Nullable;
import android.view.View;
import android.view.ViewGroup;

import com.bluelinelabs.conductor.ControllerChangeHandler;
import com.bluelinelabs.conductor.changehandler.AnimatorChangeHandler;

public class LeftHorizontalChangeHandler extends AnimatorChangeHandler {

    public LeftHorizontalChangeHandler() { }

    public LeftHorizontalChangeHandler(boolean removesFromViewOnPush) {
        super(removesFromViewOnPush);
    }

    public LeftHorizontalChangeHandler(long duration) {
        super(duration);
    }

    public LeftHorizontalChangeHandler(long duration, boolean removesFromViewOnPush) {
        super(duration, removesFromViewOnPush);
    }

    @Override @NonNull
    protected Animator getAnimator(@NonNull ViewGroup container, @Nullable View from, @Nullable View to, boolean isPush, boolean toAddedToContainer) {
        AnimatorSet animatorSet = new AnimatorSet();

        if (isPush) {
            if (from != null) {
                animatorSet.play(ObjectAnimator.ofFloat(from, View.TRANSLATION_X, from.getWidth()));
            }
            if (to != null) {
                animatorSet.play(ObjectAnimator.ofFloat(to, View.TRANSLATION_X, -to.getWidth(), 0));
            }
        } else {
            if (from != null) {
                animatorSet.play(ObjectAnimator.ofFloat(from, View.TRANSLATION_X, -from.getWidth()));
            }
            if (to != null) {
                // Allow this to have a nice transition when coming off an aborted push animation
                float fromLeft = from != null ? from.getTranslationX() : 0;
                animatorSet.play(ObjectAnimator.ofFloat(to, View.TRANSLATION_X, fromLeft + to.getWidth(), 0));
            }
        }

        return animatorSet;
    }

    @Override
    protected void resetFromView(@NonNull View from) {
        from.setTranslationX(0);
    }

    @Override @NonNull
    public ControllerChangeHandler copy() {
        return new LeftHorizontalChangeHandler(getAnimationDuration(), removesFromViewOnPush());
    }
}

```

