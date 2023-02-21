# Wave Presentation
A NodeCG bundle I created to present my [Wave Equation](https://github.com/oneirocosm/webgpu-wave-eqn) project at the [Recurse Center](https://www.recurse.com/)

As this is built with NodeCG, this presentation provides a series of background slides that can be loaded into OBS (or any form of broadcast software that can load html).  Furthermore, it provides a dashboard that can be used to control some CSS animations for these backgrounds that provide a more dynamic experience.

If you would like to see a video of the presentation, you can find it within the [main project's README page](https://github.com/oneirocosm/webgpu-wave-eqn).  If you would like a higher quality version of it, you can download it [here](https://github.com/oneirocosm/webgpu-wave-eqn/blob/main/WavePresentation.mp4)!

## A Caveat
This project should be considered a proof-of-concept more than anything else.  Since it was a project that was made for a singular use, there are a lot of unnecessary repetition and very few optimizations.  This, in turn, reveals a lot of potential pitfalls with using software such as NodeCG to develop a presentation.  That is not to say that it should be 100% avoided, but care needs to be taken to avoid these limitations.  In an ideal world, it would be possible to create a library that can sidestep these issues, and if that is true, hopefully this is a step to reaching that point.

## How to Use
With all of that out of the way, if you'd like to run this on your own, there are a few things you'll need first:
- [OBS (Open Broadcast Software)](https://obsproject.com/)
- [NodeCG](https://www.nodecg.dev/)

Theoretically, it should be possible to run this with different types of broadcast software, but that has not been tested.  Also, I ran this with the beta build of NodeCG 2.0 which worked great for me.  That being said, nothing that I actually used in the repository should limit you to the 2.0 version&mdash;the 1.9 version should work as well.

With that out of the way, there is some additional setup required as well.
1. (optional) I highly recommend creating a new **Scene Collection** in OBS for this project as it contains many scenes.
2. Create a scene to use as a **Template Scene**.
3. In this **Template Scene**, create a **Webcam Source** that you will position in the upper right quarter of the screen.
4. Copy the **Template Scene** for each slide (11 slides).  I named mine
   1. 01-title
   2. 02-equation
   3. 03-discretize
   4. 04-webgpu
   5. 05-data
   6. 06-clicks
   7. 07-walls
   8. 08-energies
   9. 09-other
   10. 10-demo
   11. 11-future
5. Within each scene, create a new **Browser Source**.  Each **Browser Source** needs to point to a url corresponding with the scene.  For this project, the urls are
   1. http://localhost:9090/bundles/wave-presentation/graphics/01-title.html
   2. http://localhost:9090/bundles/wave-presentation/graphics/02-equation.html
   3. http://localhost:9090/bundles/wave-presentation/graphics/03-discretize.html
   4. http://localhost:9090/bundles/wave-presentation/graphics/04-webgpu.html
   5. http://localhost:9090/bundles/wave-presentation/graphics/05-data.html
   6. http://localhost:9090/bundles/wave-presentation/graphics/06-clicks.html
   7. http://localhost:9090/bundles/wave-presentation/graphics/07-walls.html
   8. http://localhost:9090/bundles/wave-presentation/graphics/08-energies.html
   9. http://localhost:9090/bundles/wave-presentation/graphics/09-other.html
   10. http://localhost:9090/bundles/wave-presentation/graphics/10-demo.html
   11. http://localhost:9090/bundles/wave-presentation/graphics/11-future.html
6. Within each scene, move the **Webcam Source** on top of the **Browser Source** so the **Webcam Source** is visible.
7. In the "Demo Scene", add a **Window Capture** source of a separate web browser running [the demo](https://github.com/oneirocosm/webgpu-wave-eqn).  This must be use a **Window Capture** as the demo (which is written in WebGPU) requires a very specific browser (Chrome Canary) to run.
8. Add a final scene for **Webcam Only**.
9. In the **Webcam Only** scene, add your already existing **Webcam Source**.
10. Check out this repository in your NodeCG installation within the "bundles" directory.
11. If you need run a command such as "yarn install" do so here.  (Whether you need to do this depends on the NodeCG installation method.)
12. Run NodeCG using "yarn run dev" or something similar.
13. Open the dashboard at the url http://localhost:9090/dashboard/
14. Drag the dashboard panels around so they are in an order that makes sense for the presentation.
15. Within each source, refresh each **Browser Source**.  It should display the correct background now.
16. (optional) It is worthwhile to test the dashboard to make sure the commands are working properly.
17. You can now start your presentation in OBS using "Start Streaming," "Start Recording," or "Start Virtual Cam."

In the dashboard, you have several things that you can interact with in each panel.
- **Notes** help keep me on track for topics that I should bring up during the presentation
- **Checkboxes** act as a way to fade in individual pieces of slides, so I can pull them up as I address them.  Checkboxes can be clicked in any order.
- **Radio Buttons** are similar to checkboxes in that they reveal pieces of slides as I continue to talk.  Unlike Checkboxes, they have a defined order to them.  A slider would be more appropriate, but it was much quicker to implement radio buttons in their place.
- **Buttons** are used for individual effects that can occur any number of times.

## Trials and Tribulations
While I would say the presentation was a success overall, there were a lot of hurdles to getting it there.  Many of these are due to some underlying limitations of NodeCG that could be addressed by incorporating other technologies.

### Verbosity
The most apparent hurdle I faced in creating this presentation was how verbose it was to create slides.  For each individual slide, I had to create
- An html file in the graphics directory
- An html file in the dashboard directory
- A JSON object in the graphics section of the bundle's package.json file
- A JSON object in the dashboard section of the bundle's package.json file
- A scene in OBS
- A browser source in OBS

In order to keep them in order, I ended up naming the files with an order in mind.  Unfortunately, this meant that if the order was ever changed, I had to update all of the above files for several slides which was extremely unweildy.  It would be ideal for this to be handled in a way where I don't have to repeat myself so often.

One thing that comes to mind is sharing a webpage for all of the slides, but that creates other technical problems that I don't know how to get around&mdash;especially with replicating the fade effect that OBS handles on its own.  While this is theoretically possible with CSS, it can easily explode into an unmanageable mess with everything in the same file.  If there was a way to use the same route for multiple webpages and then to switch between them without any hard breaks, that would be perfect, but it is not possible to my knowledge.

### Repetition
I went into this a little bit in the above point, but there were many cases where I was being repetitive and didn't need to be.  A lot of this is my fault because I was so focused on getting this out the door.  For instance, a significant amount of the JavaScript used for handling replicants should be consolidated into a common file and imported into the html pages.  Additionally, enabling SASS would allow me to use mixins to abstract away a lot of the opacity animations into something much more reusable.  Lastly, the html itself has a great deal of repetition in the form of boilerplate&mdash;it would be well served by creating a template of some sort.

Most of these issues could be solved by integrating frameworks and libraries into the project.  While a framework would not be necessary for the JavaScript consolidation, it would make it a little easier and enable html templating as a nice bonus.  Not to mention, most frameworks make SASS integration trivial.  In retrospect, a lot of the local CSS had been written that way since I had been using Svelte recently.  Unfortunately, I did not figure out a quick way to integrate NodeCG with Svelte quickly enough to use it here, but I would love to do so in the future.

## Lessons Learned
With that in mind, I can narrow down some of the key takeaways of this project to this:
- Deciding on an ordering for files too early in the pipeline can cause problems down the line.  Instead, the order should be defined in one place, which in this case, would be in OBS itself.  That ensures future changes involve minimal name updates and prevents potential conflicts between different ordering conventions.
- Frameworks and SASS make things significantly easier to write.  It had been a while since I had done pure HTML/CSS/JavaScript, and I have to say, it was a lot more work than I remember it being.
- CSS Animations are incredibly powerful but can stack up boilerplate quickly.  Be careful not to make them too complicated.
- TypeScript makes debugging significantly easier.  I really missed having it here.
- A lot of modern frameworks use the file-system routing to cut down on boilerplate.  This may sound like a small thing at first, but this has made it clear to me that it's a much bigger deal than a first glance would suggest.
- Unique html files for scenes may be the best way to go right now.  Cramming everything into one file is tempting because it would cut down on repetition, but it comes with the drawback of making that file extremely complicated.  As a compromise, templating can alleviate some of the pain points without becoming unweildy.
