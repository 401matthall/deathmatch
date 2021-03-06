NOTES
=================

August 28, 2012
--------------
Viewed 52 Generations starting from scratch. 64 Organsims, 8 species.Matches take too long. Reduce the match length to 500 or 600. There is one killer species. It's fast moving snake of triangles, and it always either KOs the opponent or bashes its own head in. Another species has adapted to a long imobile snake. It either falls on or near the oppenent and does a little damage before being TKOed.Presumably this species has no capacity for movement, so the extra damage points are enough to drive it's evolution.

Most of the other species are similar to organisms found after the 5 generation. There is very little variety. They do seem to have altered slightly. Most remain very ornate. A few species never make it past the first round. I wonder if this is preventing them from receiving sufficient selection pressure to evolve (i.e. they are so far behind that small improvements in fitness fail to have any benefit). Tools to analyze a species over generations (phenotypically and genotipically) should be a high priority. 

Still no plan for speciation. Sibling species should be driven apart by competition over resources. In this simulation, resources are organisms from other species. I would like to see them alter which species they compete best against which should drive morphologic change. Perhaps they can somehow be allocated breeding spots based on which species they do best against. The way speciation is implemented is that dividing a species will force it into matches with its former siblings. This will probably the largest source of selection pressure. This will likely increse the diversity, but it will be difficult to distinguish change driven by this new source of pressure from change driven by the new genetic incompatibility.

Extinction is just as much a problem. Some species can always be expected to do miserably. If extinction is based on perfect failure, then we can expect species to go extinct all the time. One posibility would to have a small probility of extinction at the end of any generation where one of the species fails to win a single match. The gap could be filled either by spliting an existing species (which might solve the speciation issue, but reduce divesity) or generating a new random species (which would increase diversity but muddle the concept of generational age).

August 29, 2012
---------------
I increased the mutation rates and also implemented a switch that turns off rendering and speeds performs more physics calculations per second. This permits a 10 fold increase in the number of generations computed in a given time. Unfortunately, there is a bug that causes the app to crash, so the overnight tests didn't yield results. I have watched a few simulations up to 20 generations. They were much more interesting than the previous one. The creatures started off mostly immobile and gradually grew more dangerous. In one simulation, it took about 10 generations before any of the creatures really started to harm each other (there generation started off with mostly neutral flex values). Once the creatures did start attacking, there were a variety of strategies which made the contest very interesting. In another simulation, some creatures were acheiving knockouts after only 4 generations. These were snakes, and by 20 generations the snakes were dominant.

No more progress on speciation.

August 30, 2012
---------------
Fixed the Box2d bug that caused the application to crash. This allowed me run 2 experiments that got over 200 generations. One was showing very interesting creatures at about 220 generations. Both simulations seem to get less interesting after that. There are fewer leaping creatures, KOs, and hard hits. The creatures seem to be getting "fuzzier" with small parts covering their surfaces. The fuzziness might be a defensive adaptation. I'm wondering if the creatures are starting to settle into some sort of equilibrium where less risky attack behaviors are advantageous. I've seen something like this in other simulations. This would probably be anwerable if I had some way of judging competitiveness between species over time.

I'm starting to feel that producing an equal number of organisms per species in each generation is a bad idea. Unfortunately, I don't have a better solution at the moment.

If settling into a boring equilibrium is a problem, it'll likely be fixed by introducing speciation and extinction. Otherwise, we can cap evolution to x number of generations (say 100), then mix in species from other simulations.

August 31, 2012
---------------
Turns out there is another bug in Box2Dweb. Both simulations crashed after 500+ generations. Watching the simulation up to that point yielded some intersting facts. It turns out the simulation doesn't naturally regress into mediocrity. I suppose I just caught it at a boring time. There were several interesting creatures in later generations. One surprise was that there doesn't seem to be any long term stability even amongst successful forms. This may be genetic driven by a mutation rate that is too high, but it also might be adapatation by other species that drives successful species to change forms. Teasing out the difference here will be an interesting research question.

One interesting form was a long rectangle with oblong pentagons at each vertex. This creature does quite a bit of leaping and is very successful because it often lands on an opponent with the sharp side of the pentagons down. Often this is an immediate knockout.

Another interesting form was a kind of tank with a hexagon (septagon?) at it's core, rotating "wheels" on the bottom vertices and long spikes on the top vertices decorated with small hanging triangles. I noticed this creature in around the 450'th generation and tracked it and it's descendants. It was quite successful even against the creature mentioned in the previous paragraph, and it won the tournament in more than one generation. It's species, however, had a lot of variation, especially around the distribution of wheels and spikes. Most of the species would have a wheel on top or some spikes on the bottom making it much less mobile. Despite the fact that the wheels on bottom creature seemed more successful, the species did not move in that direction. Interestingly, the species did seem to coalesce, but into a creature with one wheel on the bottom and one on the top. This variant rarely made it into the semi-finals. The failure of the wheels on bottom creature could be due to a mutation rate that's too high, or perhaps due to insufficient reward for winning.

If the breeding algorithm is correct, you get these breeding combinatoins: 1:2, 1:3, 1:4, 1:5, 2:3, 2:4, 3:4, 4:5. Note that, if the 2nd and 3rd have an identical genome. If the the first has a new adaptation, we expect it to show up in a quarter of the next population. This seems reasonable, but it still might be worth looking into. It might be better to weight reproductive success on wins rather than rankings.

September 4, 2012
-----------------
Have breakpoint in place to catch box2d infinite loop error. Unfortunately, the breakpoint slows down the simulation to the point that it could take days to reproduce the error (I'm on the 3rd day now). I have basic localStorage based checkpointing in place now, so longer simulations should be possible. We're also storing enough information about matches that detailed data analysis should be possible. Haven't run enough simulations to discover anything of interest.

September 8, 2012
-----------------
I've implemented a new breeding algorithm that allocates half the slots in the next generation based on wins in the smaller wins of the tournament and the rests allocated equally amongst the species. It's implemented in such a way that the balance of prize vs. distributed slots can be adjusted by constants. I've also change the way slots allocated to an organism are filled. Before the top creature would breed with the 2nd, 3rd, 4th. etc. creatures and the 2nd ranked creature would breed with the 3rd, 4th. etc. on down. Now the organism's mate is randomly selected from the species. This seemed more natural with the addition of slots allocated by wins, but I'm not sure how it'll play out in the simulation.

I dropped mutation rate way down (from .1 to .02) to see what affect more genetic stability would have on the simulation on the long term. After ~500 generations I'm seeing mostly competive creatures, though none seem especially dangerous. This might be an adaptation to risk avoidance and it might be constrained variability due to the low mutation rate. Will have to check again later to see if the population seems more stable. Really need some analysis tools to understand the effecs of these changes.

The simulation now supports storage in a Riak database. It turns out you only get 5M of local storage and that's only enough to store about 50 generations of data. Riak should make it fairly easy to run the types of analysis queries we're looking to run.

TODO:

1. Discover and fix box2Dweb infinite loop.
2. Disqualify creatures whose initial geometry would put them in the wall or on the other creature's side.
3. Info on current opponents.
4. Start, stop, name simulation, export, acceleration.
5. Species analysis tools.
6. Tools to analyze competitiveness between species
7. KO bonus based on match length (shorter match = bigger bonus).
8. Create a simulation object that specifies paramters like generation size and mutation rate.
9. Web workers.
