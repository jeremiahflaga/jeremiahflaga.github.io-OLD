<div class="">
  <div class="float-right">
    <span class="text-primary experience-date">March 2 - 4, 2016</span>
  </div>
  <div class="">
    <h3 class="mb-3">Enlightenment Period</h3>
    <div class="col-md-10">
      <p>
        I read and studied Uncle Bob Martin's blog post titled <a  href="http://blog.cleancoder.com/uncle-bob/2016/01/04/ALittleArchitecture.html">"A Little Architecture"</a>.
      </p>
      <p>
        <em><strong>I would say that this is the time of my enlightenment on how to structure software projects.</strong></em> 
      </p>
    </div>
  </div>
</div>


<div class="col-md-10 accordion mb-5 mt-2 d-print-none" id="experience-3-enlightenment-accordion">
  <div class="card">
    <div class="card-header p-0" id="experience-3-enlightenment-heading-lessons-learned">
      <p class="mb-0">
          <button class="btn btn-link btn-block text-left collapsed subheading-small" type="button" data-toggle="collapse" data-target="#experience-3-enlightenment-collapse-lessons-learned" aria-expanded="false" aria-controls="experience-3-enlightenment-collapse-lessons-learned">
          Lessons Learned:
          </button>
      </p>
    </div>
    <div id="experience-3-enlightenment-collapse-lessons-learned" class="collapse" aria-labelledby="experience-3-enlightenment-heading-lessons-learned" data-parent="#experience-3-enlightenment-accordion">
      <div class="card-body">
        <div class="pr-3">
            <blockquote>
              "Architecture is <strong>not</strong> about frameworks and databases and web servers..."
            </blockquote>
            <p>
              Before this period, I had been thinking that software architecture is about how to combine all these frameworks and libraries and tools together to form an application!
            </p>
            <div>
              <ul>
                <li>
                  <p>
                    <small>
                      <em>Windows Forms for presentation | ADO.NET for data access | SQL Server for database | etc.</em>
                    </small>
                  </p>
                </li>
                <li>
                  <p>
                    <small>
                      <em>ASP.NET Web MVC for presentation | Entity Framework for data access | etc. </em>
                    </small>
                  </p>
                </li>
                <li>
                  <p>
                    <small>
                      <em>Hot Towel SPA Template for frontend | ASP.NET Web API for backend | Entity Framework for data access | StructureMap as DI Container | SQL Server for database | etc. </em>
                    </small>
                  </p>
                </li>
              </ul>
            </div>
            <p>
              ... have I been putting <em>much</em> of my time and attention in the wrong places!?... I'm not sure. Perhaps this is just part of growing up as a programmer through trial-and-error, with no one <em>personally</em> guiding you. :grin:
            </p>
            <blockquote>
              "The important decisions that a Software Architect makes are the ones that allow you to <strong>NOT</strong> make the decisions about the database, and the webserver, and the frameworks." &mdash; Uncle Bob Martin
            </blockquote>
            <p>
              Of course I understand that the peak of an enlightenment comes from a series of little enlightenments. So even though this article of Uncle Bob is very special to me, I acknowledge that there are lots of <em>other</em> materials (and experiences) that helped me come into this kind of enlightenment.
            </p> 
            <p>
              I have always been in search for the best way to structure software systems. Now I think I already know the gist of it:
            </p>
            <p>
              <strong>Just do your best to separate the business rules from the other parts of the software system.</strong>
            </p>
            
<div markdown="1">

Or, to use the words from the blog post ["Hexagonal != Layers"](https://tpierrain.blogspot.com/2016/04/hexagonal-layers.html) by Thomas Pierrain:
  
> divide our software in 2 distinct regions:
<br />
> **the inside** (i.e. the business application logic)
<br />
> and
<br />
> **the outside** (i.e. the infrastructure code like the APIs, the SPIs, the databases, etc.).

We have to [separate policy from mechanism](http://craftsmanshipcounts.com/policy-mechanism-preservation-business-value/) in our software systems.

Or from the words of Uncle Bob Martin himself in his book ["Clean Architecture: A Craftsman's Guide to Software Structure and Design"]():

> **All software systems can be decomposed into two major elements: policy and details**. The policy element embodies all the business rules and procedures. The policy is where the true value of the system lives.
>
> The details are those things that are necessary to enable humans, other systems, and programmers to communicate with the policy, but that do not impact the behavior of the policy at all. They include IO devices, databases, web systems, servers, frameworks, communication protocols, and so forth.  

</div>
        </div>
      </div>
    </div>
  </div>
</div>


<!-- 
From Clean Architecture book


-->

<!-- 
from http://craftsmanshipcounts.com/policy-mechanism-preservation-business-value/
 separate policy from mechanism
 
  -->