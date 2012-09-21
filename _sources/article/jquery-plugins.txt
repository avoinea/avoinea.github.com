===========================================
Writing solid and extensible jQuery plugins
===========================================
Personally, I don't like the way `jquery.com`_ encourages you to write plugins,
as it's easy to end up with a large spaghetti plate even if you're not
living in Italy :)


Despite the fact that I am a Python geek and Python is about doing things
neatly and quickly with no more than one namespace - just kidding - I still
like classes, instances, interfaces, utilities and adapters - this may be due
to the fact that for the past 7 years I’ve worked mainly with Zope and
Zope Components.


Thus, after 4 years of adding life to my work by using **JavaScript** I’m ready
to show you a very powerful way of **writing JavaScript code** and hence,
**jQuery plugins**.


First of all let me share with you the following statement from
**The Zen of Python**, by Tim Peters:

.. NOTE::
   Namespaces are one honking great idea -- let's do more of those!


Why not also apply this when **writing JavaScript code**? Here is how we start:
we define our **top namespace**, safely, in order to allow it to be extended
later outside this JS file:

.. code-block:: javascript

  if(!window.Alien){
    var Alien = {"version": "1.0"};
  }


At this point, we begin to add our plugins:

.. code-block:: javascript

  Alien.Ship = function(context, options){
    var self = this;
    self.context = context;

    self.settings = {
      width: 800,
      height: 600
    };

    if(options){
      jQuery.extend(self.settings, options);
    }

    self.initialize();
  };


Now that we've added our constructor, let's make it possible to instantiate
it with the **new** JS keyword:

.. code-block:: javascript

  Alien.Ship.prototype = {
   initialize: function(){
     var self = this;

     // Bind some events
     self.context.bind('width-changed', function(evt, data){
       self.handle_width(data);
     });

     self.context.bind('height-changed', function(evt, data){
       self.handle_height(data);
     });

     // Do some initialization
     self.context.css('background-color', 'gray');
   },


   handle_width: function(data){
     var self = this;
     self.settings.width = data.width;
   },

   handle_height: function(data){
     var self = this;
     self.settings.height = data.height;
   }
 };


OK, but where is the **jQuery plugin**? Well, here it is:

.. code-block:: javascript

  jQuery.fn.AlienShip = function(options){
    return this.each(function(){
      var context = jQuery(this);
      var ship = new Alien.Ship(context, options);
      context.data('AlienShip', ship);
    });
  };


By using this, we have the power of a **jQuery plugin** mixed with the
flexibility of the **prototype object** from JavaScript.


It's **easy to call** it:

.. code-block:: javascript

  jQuery('.car').AlienShip({width: 1024, height: 800});


It **easy to access** it:

.. code-block:: javascript

  jQuery('.car').data('AlienShip').settings.width;


And it's **easy to extend** it:

.. code-block:: javascript

  Alien.Ship.prototype.handle_width = function(data){
    var self = this;
    self.settings.width = data.width / 2;
  };


`Download the full javascript file`_


.. _`jquery.com`: http://docs.jquery.com/Plugins/Authoring
.. _`Download the full javascript file`: https://gist.github.com/3717757
