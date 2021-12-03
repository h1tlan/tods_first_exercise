# In the name of God.

```code
The message alphabet M is the set of triples consisting of a UID, a flag 
value in {out, in}, and a positive integer hop-count. 

For each i, the states in statesi consist of the following components:

    u, of type UID, initially i's UID 
    send+, containing either an element of M or null, 
        initially the triple consisting of i's UID, out, and 1 
    send-, containing either an element of M or null, 
	initially the triple consisting of i's UID, out, and 1 
    status, with values in {unknown, leader}, initially unknown 
    phase, a nonnegative integer, initially 0
    right, with values in {unknown, leader}, initially unknown
    clock, initially 0

The set of start states starti consists of the single state defined by the 
given initializations. 

For each i, the message-generation function msgsi is defined as follows:

    clock := clock + 1
    if clock < 2^phase  then    
	send the current value of send+ to process i + 1
	send+ := null

    else
	send the current value of send- to process i - 1
	send- := null
	

For each i, the transition function transi is defined by the following pseudo_code: 

    if the message from i- 1 is (v, out, h) then 
	case
	    v > u and h > 1: send+ := (v, out, h- 1) 
	    v > u and h = 1: send- :- (v, in, 1)
	    v = u: status := leader 
	endcase 

    if the message from i+1 is (v, out, h) then
	case
	    v > u and h > 1: send- := (v, out, h -1)
	    v > u and h = 1: send+ := (v, in, 1)
	    v = u: status := leader
	endcase

    if the message from i - 1 is (v, in, 1) and v != u then 
	send+ := (v, in, 1)

    if the message from i + 1 is (v, in, 1) and v != u then 
	send- := (v, in, 1) 

    if the message from i + 1 is (v, in, 1) and v == u then 
	right := leader

    if the message from i - 1 is (u, in, 1) and right == leader
	phase := phase + 1
	clock := 0
	right := unknown
	send+ := (u, out, 2^phase)
	send- := (u, out, 2^phase)
```

## Stuck after 5 step or clock in process 3, (i = 3).
