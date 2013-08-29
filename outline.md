# BDDing your Infrastructure with Vagabond

* Background
    * In a previously life... Cowboy Sysadmin
        * We had loads of Xen VMs all managed by hand.
        * I was pretty good with the duct tape + SVN in order to keep things
          running
        * Found out about Puppet from @ckdake but it's hard and the team was
          _very_ resistant.
    * Demandbase
        * Came to Highgroove and joined a crazy big analytics problem with a
          ton of AWS Infrastructure.
        * Lots of Chef and I was bad with it.
        * Even when I got better with it spinning up new hosts would not always
          work out of the box... 
        * And never on a Saturday when I was alone...
        * "Testing" all of our recipes was a) vague and b) not an option.
        * and it made me work/risk averse!
* What do you mean by BDD?
    * Probably not what you do...
    * I care that not only are my chef or puppet configurations "correct" but that
      they actually do the right thing...
    * So what is "the right thing" in this case?
        * Packages are installed
        * The correct _version_ of packages are installed.
        * Other packages that should be uninstalled actually _are_
          uninstalled...
        * Services that should be reachable are actually reachable...
        * Those services are actually running the correct versions!
        * Deployed applications are actually working!
* The competition - a literature survey.
    * I'm not the only one to have felt this pain clearly.
        * Last year at ArrrrCamp Bernard Grymonpon gave a great talk on [Testing
          Chef Recipes](http://vimeo.com/51911896) -
          [Deck](https://speakerdeck.com/wonko/arrrrcamp-testing-chef)
        * [Foodcritic](http://acrmp.github.io/foodcritic/) - a Linter which
          seems pretty cool but isn't a tester..  more of a style-guide.  Seems
          like a Good Thing.
        * [Rspec-chef](https://github.com/calavera/rspec-chef) - last commit Jan 11, 2012
        * [chefspec](http://acrmp.github.io/chefspec/) - "ChefSpec runs your
          cookbook but without actually converging the node that your examples
          are being executed on." Also seems cool but doesn't meet my needs!  To
          quote Bernard's talk "feels like testing chef, not the recipe"
        * [minitest-chef-handler](https://github.com/calavera/minitest-chef-handler)
          seems a lot like what I was shooting for!
* Vagabond
    * As usual, anything I do well someone else actually wrote and I enhance a
      tiny amount.  Profit!
    * Originally written by @wfarr during his RailsMachine tenure
        * When I had this idea I bounced it off @alindeman and @vanstee and the
          latter told me about Vagabond.
        * When I found it there was a pretty extensive TODO list, and it totally
          didn't do what I really wanted
        * I struggled to get it working... and I struggled to understand how it
          worked
        * but it was something!
        * and finally I had an ah-ha moment - specs run from the home directory!
        * Then I checked off some of the TODO list (there are some hard problems
          in there to do in a general way!)
    * What I added
        * File "should contain" - actually a lot harder than it sounds
            * If you just want a string on a single line `file.should contain 'bar'`
              it's not so bad but could be super slow for big files
            * What about regexps?  What about multiline regexps?  On HUGE files?
            * Hard to do "right" (hard to define "right"!)  Not so terrible if you
              are willing to do something and fail...
        * a `portscan` module to scan ports and services (via ruby-nmap) in order
          to determine what's actually running.
            * `should have_open_port 22`
            * `should only_have_open_ports [22, 443]`
            * `should have_open_port "ssh"`
            * `should have_remote_service "OpenSSH", on_port: 22`
            * `should have_remote_service_version "OpenSSH", "5.3p1"`
        * a "webapp" module
            * Actually wraps up a remove capybara-webkit instance
            * Write capybara specs and run them against a service that is deployed
              in the Vagrant instance!
        * "upgrade paths"
            * So your infrastructure is on v1 of your cookbooks, and you want to
              upgrade to v2 and know that it works...
            * We can do that with git tags (though it's kinda Hacky/PoC right
              now)
    * What needs to be done...
        * What you can't do now...
           ```
           ~/some_project_with_vagrant$ cat Gemfile | grep vagabond
           gem 'vagabond'
  
           ~/some_project_with_vagrant$ vagabond
           # here all your tests verify what's in your Vagrantfile
           ```
  
          ```
          ~/some_project_with_vagrant_and_multiple_hosts_it_spins_up$ vagabond
          # here all of your tests run _per_ Vagrant host spun up!
          ```

# Good Beer

* Saison - Saison Dupont
* Gueze - Drie Fonteinen Oude Gueze
* Trappist - Westmalle Trippel
* Abbaye - Grimbergen Blonde
* Tripel - Tripel Karmeleit
* Flemish Red / Oud Bruin - Rodenbach Grand Cru
* Westvleteren - Everything
