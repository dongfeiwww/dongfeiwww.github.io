---
layout: post
title:  "Interview Question: Parking Lot"
date:   2014-09-27 20:30:29
categories: code
---

# Interview Question: Parking Lot

## Basic Question
---------
	I'm the owner of a parking lot empire. I'm still pretty small - I only have 3 lots in downtown, but my visions are grand. As such, I've hired you to design and build an automated system for managing my parking lots. Assume that you have access to all the latest in ticket-giving and ticket-accepting machines, a sensor network built into the parking lot, and programmable signs to give directions to customers. I want to charge customers by the amount of space they use in the lot (area) and the length of time they use the lot (time).


A good candidate should start asking you questions at this point, determining the bounds of the problem and getting clarifying information (this is good). But if not, help focus them on a couple specific use cases:

   * To track charges for each car: when each car comes in, they get a ticket and an automated vehicle measurement (to determine size of vehicle); when they leave, they submit the ticket and are charged accordingly.


   * have the parking lot automation system manage available space to maximize usage/profit (e.g. direct customers to where they should park)


   * "Lot Full" signs with Internet connections that can query the system as well; they should turn on whenever each lot is full.

The candidate should then dive into the problem and start scoping out objects and methods and such. Some candidates with DB backgrounds tend to go towards DBs and tables, others stay at the OO level. It's up to the interviewer to either tell the candidate ahead of time which version they'd like the candidate to do or just accept either solution.

Checklist of things that the candidate might design:

   * Object hierarchy
   * Objects for vehicles, sensors, control system, billing system, space-planning system, customer notifications
   * Algorithms to interface with sensors, control system, billing system, space planning
   * Database schema design

### Want more?

If you have more time, there are a number of places to move on from here, like:

   * (works better with DB model) Tell the candidate that you've been doing really well and it's 2 years later and you now have 1,000 lots across the entire country and the system no longer works under the load. How would they modify the architecture to accommodate this increased load? (Hopefully they get into some type of distributed architecture at which point concurrency issues, partitioned data, and such come in, if not you can bring them up).


   * Tell candidate you'd like to run a number of reports on the data to analyze usage. One particular report is to find any time periods that have incredibly high short-term load (e.g. a local play or sporting event) where you might want to consider special pricing.


   * Speaking of special pricing, if they haven't already done so, ask them how they could modify their design to support a myriad of pricing models: daily, weekly, hourly, early bird, special event, etc. And, if they don't think of this themselves, ask them how they deal with cars that "leak" across multiple pricing models (e.g. a car that stays for a week that overlaps a special event)?

### Calibration
-----------

Here are some general thoughts on how I calibrate answers.

   * A good engineer will take the initial intro and immediately start asking clarifying questions ranging from "how much time does my team have to create this" to "how many different pricing models do you want to accept" to "how reliable a system (uptime) do you want this to be".


   * A good engineer will create (at some point in the process) his own more complete set of use cases and especially edge conditions (e.g. "what happens when two people enter the same lot from different entrances at the same time and there's only one spot left"). Often for the purpose of the interview, you'll say, "don't worry about that" (due to time constraints), but the fact that they ask about that stuff is good.


   * A good engineer will create a solution off the bat that easily supports logging and report generation (and anyone who's been around the block once or twice will probably assume you want to lock transaction history).


   * A good solution will not require re-architecture going from a 2 payment system to an n-payment system. (A good componentized solution is always better.)

### Additional note
----


   * What to look for? Expected answer: A parking lot is (at least conceptually) a container for objects of type vehicle - vehicle may be subclassed for different kinds of vehicles: trucks, motorcycles, cars, ... - Important concepts: container, inheritance!
   * Does the candidate jump to conclusions as to what the parking lot is supposed to do?
   * Does the candidate approach this problem in an object-oriented way or in a data-driven or functional way?
   * After the first general stab, the candidate should ask for additional requirements: what is the parking lot supposed to do, how is it to be used. Does the candidate offer possible ways that it might be used (in a fitting problem, in a real-time control system, in a billing problem, for instance)
   * Comments about implementation issues are generally inappropriate, unless the specific use of the parking lot has been established!
   * Follow-up question: What functionality would be abstracted/encapsulated in the parking lot? Expected answer: size or capacity, occupancy, even which slots are filled. Anything else depends on the specific usage!
   * Advanced follow-up question: where should the responsibility to to park a vehicle? ParkingLot.park( Vehicle ) or Vehicle.park( ParkingLot )? Reason for choice? Introduce Meilir Page-Jones example: unmilk the cow, or uncow the milk? Therefore: Introduce parking mgr (Can the candidate name an applicable Pattern? Mediator!)
      * Benefits: less smart objects: more reusable
      * less coupling (otherwise cars need to know about parking lot and vv)
      * Single point of control
      * Better conceptual modeling!!!
