### Is it OK to use iPerf TCP tests?   
# NO!

iPerf TCP tests are an ineffective and essentially meaningless test of network performance when latency is anything but trivial.  

TCP needs very many things to all work right to deliver adequate performance.  When it works, it proves that things work, and that's swell.  But when it *fails* you learn next to nothing about how or why.  

## Do's:
1. Use UDP instead of TCP  
    * Lots of things have to go right for TCP to accurately measure performance.   UDP just throws packets at the network as fast as you configure it to.  This is what you need. 

2. Use Multiple streams!  And/or multiple concurrent tests.  And/or multiple concurrent pairs of hosts running tests!  
    * generating lots of traffic can be kind of hard sometimes and you probably don't want to spend your time troubleshooting the behavior of iPerf.   You want to troubleshoot the nework so stop tuning iPerf and just generate as much traffic as possible already.

3. ignore "end to end" performance until you've verified the individual components of the path.  
    * The only thing worth fixing is the weakest link, so put all effort into finding that first.

4. use interface counters to validate network components.  
    * In this diagram I would mean the RX and TX counters of the Ethernet1/1 interfaces of ASW1 and ASW2
        * Compare the TX at SiteA with the RX at SiteB and *vice versa* 

    * The circuit of course can never deliver more packets than you transmit, so to test the circuit make sure you're *actually* transmitting enough data, then confirm you're receiving *about* the same amount on the other side
    
    * These counters are themselves averages and they'll never exactly match.  It's a good idea to configure the `load-interval` if you can to be nice and tight (30 seconds, etc) so you can see quickly how much actual data is passing, and then look for them to be *approximately* the same between sites ![here](/docs/assets/images/base_setup.png)

## If you are running iPerf to test a circuit or any other kind of network path that's long enough that it leaves your building, you MUST use UDP!

* Even UDP tests 