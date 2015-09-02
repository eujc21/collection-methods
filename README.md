# Collection-Methods


----------


####**Objective**:
>Adding Methods to your MeteorJS Collections


----------


#### **Installation**
>	meteor add patrickml:collection-methods


----------


#### **Purpose**
>This idea came about when confronted with the issue of using repetetive queries in method calls.  i.e. something like the following

	createCharge: function (deviceId, config) {

        var items = Cart.find(
        	{
        		session: invoiceId,
	        	item:{$exists:true},
	        	invoiceId: {$exists: false},
	        	couponId: {$exists: false}
	        }
	      ).fetch();


>This is  something we wish to avoid, for repetetive code gets old real quick.


----------

####**Relieve the Headache, avoid typing as much.**
>The following is an example of how you would set up the method in your collections file.

    Posts = new Meteor.Collection('listings');

    Posts.methods({
    /**
       * Find all `Posts` that belong to the current user.
       * @param query
       * @returns {*}
       */
      findWithUser: function(query) {
        var userId;
        userId = Meteor.userId();
        if (!userId) {
          throw new Meteor.Error('NO USER LOGGED IN', 422);
        }
        if (typeof query === 'undefined') {
          query = {};
        }
        query.userId = user;
        return this.find(query);
      },
      /**
       * Find all `Posts` where tags exist.
       * @param query
       * @returns {*}
       */
      findWhereHasTags: function(query) {
        if (typeof query === 'undefined') {
          query = {};
        }
        query.tags = {
          $exists: true
        };
        return this.find(query);
      },
      /**
       * Find all `Posts` with a set of tags.
       * @param tags
       * @param query
       * @returns {*}
       */
      findWithTags: function(tags, query) {
        if (typeof query === 'undefined') {
          query = {};
        }
        query.tags = {
          $in: tags || []
        };
        return this.find(query);
      }
    });




----------

####**Usage**
>Now the 'Collection-Method' can be called  in your methods to perform a query.

	'something':  function (param) {
	        var postWU = Post.findWithUser({ _id : param})
	 }
