# activerecord-import

activerecord-import is a library for bulk inserting data using ActiveRecord.

### Modification

I have modified activerecord-import to revert to upserts with the Upsert gem, when (only) a `:taken` validation fails for that object. This means that where they are records that are deemed to already exist, Upsert will update the columns with your fresh data. This is an improvement in my view to using ON DUPLICATE KEY UPDATE, because it doesn't rely on database level unique constraints - it uses the model's validations.

Word of warning - due to my ameteurishness, this gem implements a monkey patch for AR validations which persists the validation type through to the error object, this is to enable me to determine, of the failed validations, which are uniqueness constraints or not. There's probably a better way to do this but what I have done seems to work for me.

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
