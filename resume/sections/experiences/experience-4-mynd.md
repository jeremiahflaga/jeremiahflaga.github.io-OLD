<div class="d-flex flex-column flex-md-row justify-content-between">
    <div class="flex-grow-1">
        <h3 class="mb-0">Software Developer (Mobile, Java, Android Framework)</h3>
        <div class="subheading mb-3">
            <a href="http://www.myndconsulting.com/">Mynd Consulting</a>
        </div>
        <p>Involved in developing versions 2 and 3 of an Android app for a TV show in the US.</p>
    </div>
    <div class="flex-shrink-0"><span class="text-primary">October 2016 - May 2018 (1 year, 7 months)</span></div>
</div>


<div class="accordion mb-5 mt-2" id="experience-4-mynd-accordion">
    <div class="card">
        <div class="card-header p-0" id="experience-4-mynd-heading-contributions">
            <p class="mb-0">
                <button class="btn btn-link btn-block text-left  collapsed" type="button" data-toggle="collapse" data-target="#experience-4-mynd-collapse-contributions" aria-expanded="true" aria-controls="experience-4-mynd-collapse-contributions">
                Contributions:
                </button>
            </p>
        </div>
        <div id="experience-4-mynd-collapse-contributions" class="collapse" aria-labelledby="experience-4-mynd-heading-contributions" data-parent="#experience-4-mynd-accordion">
	        <div class="card-body col-md-9">
                <div class="pr-3 border-right border-light">
                    <p>
                        Nothing extraordinary. Just finished the tasks assigned to me.
                    </p>
                    <p>
                        But this time, because I already have a better understanding of polymorphism (and its use), the SOLID principles, and other things related to software design, I tried to use these  knowledge in my coding.
                    </p>
                </div>
            </div>
        </div>
    </div>
    <div class="card">
        <div class="card-header p-0" id="experience-4-mynd-heading-lessons-learned">
	        <p class="mb-0">
	            <button class="btn btn-link btn-block text-left  collapsed" type="button" data-toggle="collapse" data-target="#experience-4-mynd-collapse-lessons-learned" aria-expanded="false" aria-controls="experience-4-mynd-collapse-lessons-learned">
	            Lessons Learned:
	            </button>
	        </p>
        </div>
        <div id="experience-4-mynd-collapse-lessons-learned" class="collapse" aria-labelledby="experience-4-mynd-heading-lessons-learned" data-parent="#experience-4-mynd-accordion">
	        <div class="card-body col-md-9">
                <div class="pr-3 border-right border-light">
                    <ol>
                        <li>
                            <p>
                                I learned how to work on a Java platform.
                            </p>
                            <p>
                                Before this job, I only knew how to do real world apps using .NET.
                            </p>
                            <p>
                                Doing Android made me more confident that I can also do work on other platforms.
                            </p>
                        </li>
                        <br />
                        <li>
                            <p>
                                Even though we were using <a href="/2018/05/23/rxjava-is-not-intuitive/">RxJava</a> in our project, which others say makes dealing with background threads much easier, there was a time where I had to deal with what they call a <em>race condition</em> with this <em>threads</em> thing while working on version 2 of the app. 
                            </p>
                            <p>
                                It was hard. The bug was intermittent. It was hard to find, partly because a large part of the project uses RxJava in a wrong way(?)
                                After I few weeks/months working on the project I discovered that when using RxJava, methods must return <code>Observables</code> all the way up from the data source to the presentation layer, so that they can be composed.
                            </p>
                            <p>
                                But I'm not 100% sure if the wrong way of using RxJava is truly the reason for that bug. Perhaps it was just my ignorance on how to deal with threads.
                                Luckily, that feature was removed during the development of version 3 of the app. <em>Yehey!</em>
                            </p>
                            <p>
                                I still have to learn more about this <em>threads</em> thing.
                            </p>
                        </li>
                    </ol>
	            </div>
	        </div>
        </div>
    </div>
</div>