# ALVR-DBMS-API

## model
All the models should have a base model:

- Base

```
Base struct {
	ID        uint `gorm:"primary_key"`
	CreatedAt time.Time
	UpdatedAt time.Time
	DeletedAt *time.Time `sql:"index"`
}

```

The `ID` item is an UUID.

- User

```
User struct {
    ...
    UserName       string
    PasswordDigest string
    Nickname       string
    Status         string  // `Admin` or `Normal User`
}
```

- Preset

```
Preset struct {
    ...
    Title  string
    Info   string
    Content  string
    Status string  // `private`, `under_review` `public`
    PresetID string // save the preset_id here
}
```

- Comment(or Chat???)

```
Comment struct {
    ...
    Message  string
    PresetID string // save the preset_id here
}
```

## apis

### POST `/api/user/register` 
parameters: 
- Nickname
- UserName
- Password
- PasswordConfirm

There are some limitations for the Nickname, UserName,Password. If there's something wrong, for example, PasswordConfirm is not the same as Password, then return error code. Note the Password should not store in the database directly, instead it should be encrypted first, and only the PasswordDigest should be stored.


### POST `/api/user/login`
parameters: 
- UserName
- Password

### POST `/api/user/logout` (need login)

Note: use `POST` here for security.

### PUT `/api/user/account` (need login)
Update the user's profile.


### POST `/api/presets` (need login)

Create a new preset.

parameters:
- Title: The name of the presets
- Info: The description of the presets
- Content: A string contains the information about the presets such as settings and codes.

### GET `/api/preset/:id`
Get/Download the preset by `preset.ID`.

### PUT `/api/preset/:id` (need login)
Update the preset by `preset.ID`.
		
### DELETE `/api/preset/:id` (need login)
Delete the preset by `preset.ID`.

### GET `/api/user/preset`
Query the presets list of the user.

### POST `/api/preset/comment/:id` (need login)
Submit a comment about a preset. Note, the `:id` here is the `preset.ID`.

### DELETE `/api/preset/comment/:id` (need login)
Delete a comment. Note, the `:id` here is the `comment.ID`.

### GET `/api/presets/comments/:id`
Comments list of a preset. Note, the `:id` here is the `preset.ID`.

### POST `/api/preset/status` (need Admin)
Admin can change the status of a preset.


