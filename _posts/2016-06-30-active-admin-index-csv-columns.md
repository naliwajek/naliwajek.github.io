---
layout: post
title: ActiveAdmin identical columns for web and CSV
category: Technical Notes
---
[ActiveAdmin](http://activeadmin.info) can define columns for index
pages, CSV documents, JSON responses and many other formats.

Here's a question of today: what if I want to display
same columns in the admin panel that I want to export in a CSV file?

Here's a simple solution using modules:

~~~ ruby
# app/admin/active_admin_user_columns.rb
module ActiveAdminUserColumns
  def use_default_columns
    column :id
    column :name
    column :age

    # multiple other columns
  end
end

# app/admin/user.rb
ActiveAdmin.register User do
  actions :index, :destroy

  index do
    status_tag "Total no of users: #{User.count}"

    extend ActiveAdminUserColumns
    use_default_columns

    column 'Actions' do |user|
      link_to 'Delete',
        admin_user_path(user),
        method: :delete
    end
  end

  csv do
    extend ActiveAdminUserColumns
    use_default_columns
  end
end
~~~

Now both the web page and the downloaded CSV file will contain same
columns. Further more, `index` was still customized with an _Actions_
column and a status tag.

## How does it work?

And why do we call `extend` two times anyway?

The thing is, those two blocks, `index` and `csv`, are processed by two different
classes. To test it just put a `binding.pry` in them and see what pry's
telling us:

~~~ ruby
> index { binding.pry }
[1] pry(#<ActiveAdmin::Views::IndexAsTable>)>

> csv { binding.pry }
[1] pry(#<ActiveAdmin::CSVBuilder>)>
~~~

And so we have it. We extend with our new module:

* `ActiveAdmin::Views::IndexAsTable` for a web page
* `ActiveAdmin::CSVBuilder` for CSV formatter

This is pure ActiveAdmin's magic.

We still can include logic in those shared columns. For example, filter
what columns get displayed by a class:

~~~ ruby
# app/admin/active_admin_user_columns.rb
module ActiveAdminUserColumns
  def use_default_columns
    column :id unless self.class == ActiveAdmin::Views::IndexAsTable

    # ...
  end
end
~~~

That's it.

DRYing up ActiveAdmin is hard because of the relations between multiple
formats but this simple modular approach can give us some basis to work
it out.
