# Ruby

## Random Shits I Care About

* Avoid using .js.erb files, use $.get, $.ajax or other jQuery shits.

* Prefer ' (single quote) over " (double quotes).

* Prefer `object` or `[args*]` over url helpers.

  Example:
  
  * use `redirect_to @user` rather than `redirect_to user_path(@user)`.
  * use `redirect_to [:admin, @post]` rather than `redirect_to admin_post_path(@post)`

* Avoid using instance variables on Observers, because Observers are fucking singletons.
