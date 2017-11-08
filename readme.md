
## migrations

users                   :   This holds the users data of the application
password_resets         :   Holds token information when users request a new password
permissions             :   This holds the various permissions needed in the application
roles                   :   This holds the roles in our application
role_has_permission     :   This is a pivot table that holds relationship information between the permissions table and the role table
user_has_roles          :   Also a pivot table, holds relationship information between the roles and the users table.
user_has_permissions    :   Also a pivot table, holds relationship information between the users table and the permissions table.

## Configuration

[ php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider" --tag="config" ]

The config file allows us to set the location of the Eloquent model of the permission and role class. You can also manually set the table names that should be used to retrieve your roles and permissions. Next we need to add the HasRoles trait to the User model:

##A role can be created like a regular Eloquent model, like this:

use Spatie\Permission\Models\Role;
use Spatie\Permission\Models\Permission;

$role = Role::create(['name' => 'writer']);
$permission = Permission::create(['name' => 'edit articles']);

##Can also get the permissions associated to a user like this:
$permissions = $user->permissions;

##Using the pluck method, pluck() you can get the role names associated with a user like this:
$roles = $user->roles()->pluck('name');

##Other Methods
Other methods available to us include:

    givePermissionTo(): Allows us to give persmission to a user or role
    revokePermissionTo(): Revoke permission from a user or role
    hasPermissionTo(): Check if a user or role has a given permission
    assignRole(): Assigns role to a user
    removeRole(): Removes role from a user
    hasRole(): Checks if a user has a role
    hasAnyRole(Role::all()): Checks if a user has any of a given list of roles
    hasAllRoles(Role::all()): Checks if a user has all of a given list of role

The methods assignRole, hasRole, hasAnyRole, hasAllRoles and removeRole can accept a string, a Spatie\Permission\Models\Role-object or an \Illuminate\Support\Collection object. The givePermissionTo and revokePermissionTo methods can accept a string or a Spatie\Permission\Models\Permission object.

#Laravel-Permission also allows to use Blade directives to verify if the logged in user has all or any of a given list of roles:
-------------------------------------
@role('writer')
    I'm a writer!
@else
    I'm not a writer...
@endrole

@hasrole('writer')
    I'm a writer!
@else
    I'm not a writer...
@endhasrole

@hasanyrole(Role::all())
    I have one or more of these roles!
@else
    I have none of these roles...
@endhasanyrole

@hasallroles(Role::all())
    I have all of these roles!
@else
    I don't have all of these roles...
@endhasallroles
-------------------------------------

The Blade directives above depends on the users role. Sometimes we need to check directly in our view if a user has a certain permission. You can do that using Laravel's native @can directive:
-------------------------------------
@can('Edit Post')
    I have permission to edit
@endcan
-------------------------------------
