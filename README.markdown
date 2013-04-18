# activerecord-import

activerecord-import is a library for bulk inserting data using ActiveRecord.

### This fork

This fork is a modification of zdennis/activerecord-import to revert to upserts with the Upsert gem, when (only) a `:taken` (validate_uniqueness_of) validation fails for that object. This means that where there are records that are deemed to already exist, Upsert will update these with your fresh data. This is an improvement in my view to using ON DUPLICATE KEY UPDATE, because it doesn't rely on database level unique constraints - it uses the model's validations. Also, the Upsert gem supports other ORMs than just MySQL.

It's an attempt to harness the power of both AR-import and Upsert.

### Example

Consider the following model:

    class Organism < ActiveRecord::Base
      validates_uniqueness_of :scientific_name
      # ... some other validations ...
    end

If we're recieving a data file of Organisms, and we want to add or update our records with what we're receiving in our data, preserving our AR validations, then we can build some Organism objects from our data, and pass them to AR-import like usual (all options work as before):

    Organism.import organisms, synchronize: organisms, synchronize_keys: [:o_id]

For all the objects that don't fail our validations, AR-import runs like usual. If an organism has the same scientific_name as an existing one, and thus failing that uniqueness validation (and only a uniqueness validation), it will be passed on to Upsert which will batch upsert all those records.

To exclude columns from being updated with the upsert, pass them to the `:except` option when calling import, f.e:

    Organism.import organisms, synchronize: organisms, synchronize_keys: [:o_id], except: [:status]

This will prevent `status` from being updated with the upsert.

**Feel free to contribute && make it better, or tell me it's unnecessary. I just build it for my purpose but I can see it being potentially useful for others.**

### Rails 3.1.x and higher

Use the latest activerecord-import.

### Rails 3.0.x up to, but not including 3.1

Use activerecord-import 0.2.11. As of activerecord-import 0.3.0 we are relying on functionality that was introduced in Rails 3.1. Since Rails 3.0.x is no longer a supported version of Rails we have decided to drop support as well.

### For More Information

For more information on activerecord-import please see its wiki: https://github.com/zdennis/activerecord-import/wiki

# License

This is licensed under the ruby license. 

# Author

Zach Dennis (zach.dennis@gmail.com)

# Contributors

* Blythe Dunham
* Gabe da Silveira
* Henry Work
* James Herdman
* Marcus Crafter
* Thibaud Guillaume-Gentil
* Mark Van Holstyn 
* Victor Costan
* Mike Campbell
