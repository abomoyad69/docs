# My Account API Contract

## Authentication

All endpoints require authentication via JWT. Include the JWT token in the Authorization header:

```
Authorization: Bearer {your_jwt_token}
```

## Endpoints

### Get User Information

Retrieves comprehensive information about the authenticated user, including profile details, languages, groups, and permissions.

- **URL**: `/my-account/v2/user-info`
- **Method**: `GET`
- **Auth Required**: Yes

#### Response

```json
{
  "userID": 123,
  "nameAr": "اسم المستخدم",
  "nameEn": "User Name",
  "username": "username123",
  "phoneNumber": "+9715012345678",
  "extensionPhoneNo": "1234",
  "email": "user@example.com",
  "imagePath": "uploads/username123.png",
  "organizationID": 5,
  "organizationNameAr": "اسم المنظمة",
  "organizationNameEn": "Organization Name",
  "userSourceID": 1,
  "userSourceAr": "مصدر المستخدم",
  "userSourceEn": "User Source",
  "userParentID": 100,
  "parentNameAr": "اسم المدير",
  "parentNameEn": "Manager Name",
  "languages": [1, 2], // Array of language IDs
  "groups": [5, 8, 12], // Array of group IDs
  "permissions": [101, 102, 103, 104] // Array of terminal action IDs
}
```

#### Error Responses

- **404**: User does not exist
  ```json
  {
    "error": "my-account.userDoesNotExist"
  }
  ```

### Update User Information

Updates user information, including profile details, password, image, languages and groups.

- **URL**: `/my-account`
- **Method**: `PATCH`
- **Auth Required**: Yes
- **Content-Type**: `multipart/form-data`

#### Request Body

```json
{
  "userInfo": {
    "nameAr": "اسم المستخدم",
    "nameEn": "User Name",
    "username": "username123",
    "email": "user@example.com",
    "phoneNumber": "+9715012345678",
    "address": "User Address",
    "userpassword": "Current-Password123", // Current password (required for password change)
    "newPassword": "New-Password123", // New password (optional)
    "confirmPassword": "New-Password123", // Confirm new password (must match newPassword)
    "imagePath": "[base64-encoded-image-data]", // Base64 image data
    "languages": [1, 2], // Array of language IDs
    "groups": [5, 8, 12], // Array of group IDs
    "userSource": true,
    "organization": 5,
    "parent": 100
  }
}
```

#### Response

```json
{
  "message": "Done"
}
```

#### Error Responses

- **400**: User does not exist
  ```json
  {
    "error": "my-account.userDoesNotExist"
  }
  ```

- **400**: Nothing to update
  ```json
  {
    "message": "my-account.nothingToUpdate"
  }
  ```

- **400**: Wrong current password
  ```json
  {
    "error": "my-account.oldPasswordWrong"
  }
  ```

- **400**: Missing new or confirm password
  ```json
  {
    "error": "my-account.newAndConfirmRequired"
  }
  ```

### Get User Role for Survey

Retrieves the user's role description for a specific survey.

- **URL**: `/my-account/:surveyID`
- **Method**: `GET`
- **Auth Required**: Yes
- **URL Parameters**: `surveyID` - The ID of the survey

#### Response

```json
{
  "roleDescAr": "وصف الدور",
  "roleDescEn": "Role Description"
}
```

#### Default Response (No Survey Selected)

```json
{
  "roleDescAr": "لايوجد لك دور, اختار استمارة اولا",
  "roleDescEn": "No Current Role, Pick a Survey First"
}
```

## Related Lookup Endpoints

### Get Organizations

Retrieves the list of all organizations.

- **URL**: `/look-up/organizations`
- **Method**: `GET`
- **Auth Required**: Yes

#### Response

```json
[
  {
    "id": 1,
    "nameAr": "اسم المنظمة 1",
    "nameEn": "Organization Name 1"
  },
  {
    "id": 2,
    "nameAr": "اسم المنظمة 2",
    "nameEn": "Organization Name 2"
  }
]
```

### Get Groups

Retrieves the list of all groups.

- **URL**: `/look-up/groups`
- **Method**: `GET`
- **Auth Required**: Yes

#### Response

```json
[
  {
    "id": 1,
    "nameAr": "اسم المجموعة 1",
    "nameEn": "Group Name 1"
  },
  {
    "id": 2,
    "nameAr": "اسم المجموعة 2",
    "nameEn": "Group Name 2"
  }
]
```

### Get Languages

Retrieves the list of all languages.

- **URL**: `/look-up/languages`
- **Method**: `GET`
- **Auth Required**: Yes

#### Response

```json
[
  {
    "lowValue": 1,
    "descAr": "العربية",
    "descEn": "Arabic"
  },
  {
    "lowValue": 2,
    "descAr": "الإنجليزية",
    "descEn": "English"
  }
]
```

### Get Containers (UI Components)

Retrieves UI components/containers for the current system.

- **URL**: `/look-up/containers`
- **Method**: `GET`
- **Auth Required**: Yes

#### Response

```json
[
  {
    "id": 1,
    "nameAr": "اسم المكون 1",
    "nameEn": "Component Name 1",
    "systemTypeID": 1
  },
  {
    "id": 2,
    "nameAr": "اسم المكون 2",
    "nameEn": "Component Name 2",
    "systemTypeID": 1
  }
]
```

### Get Terminal Actions

Retrieves all available terminal actions.

- **URL**: `/look-up/terminal-actions`
- **Method**: `GET`
- **Auth Required**: Yes

#### Response

```json
[
  {
    "id": 101,
    "nameAr": "اسم الإجراء 1",
    "nameEn": "Action Name 1",
    "containerID": 1,
    "actionTypeID": 1,
    "containerNavigateToID": 2
  },
  {
    "id": 102,
    "nameAr": "اسم الإجراء 2",
    "nameEn": "Action Name 2",
    "containerID": 1,
    "actionTypeID": 2,
    "containerNavigateToID": 3
  }
]
```

## Data Models

### User Information

| Field | Type | Description |
|-------|------|-------------|
| userID | number | Unique identifier for the user |
| nameAr | string | User's name in Arabic |
| nameEn | string | User's name in English |
| username | string | User's username |
| phoneNumber | string | User's phone number |
| extensionPhoneNo | string | User's extension phone number |
| email | string | User's email address |
| imagePath | string | Path to user's profile image |
| organizationID | number | ID of the user's organization |
| organizationNameAr | string | Organization name in Arabic |
| organizationNameEn | string | Organization name in English |
| userSourceID | number | ID of the user source |
| userSourceAr | string | User source in Arabic |
| userSourceEn | string | User source in English |
| userParentID | number | ID of the user's parent/manager |
| parentNameAr | string | Parent/manager name in Arabic |
| parentNameEn | string | Parent/manager name in English |
| languages | number[] | Array of language IDs the user knows |
| groups | number[] | Array of group IDs the user belongs to |
| permissions | number[] | Array of terminal action IDs the user has access to |

### Update User Information

| Field | Type | Description | Required |
|-------|------|-------------|----------|
| nameAr | string | User's name in Arabic | No |
| nameEn | string | User's name in English | No |
| username | string | User's username | No |
| email | string | User's email address | No |
| phoneNumber | string | User's phone number | Yes |
| address | string | User's address | No |
| userpassword | string | Current password (needed for password change) | No |
| newPassword | string | New password | No |
| confirmPassword | string | Confirm new password (must match newPassword) | No |
| imagePath | string/Base64 | Profile image (can be Base64 encoded data or existing path) | No |
| languages | number[] | Array of language IDs | No |
| groups | number[] | Array of group IDs | No |
| userSource | boolean | User source flag | No |
| organization | number | Organization ID | No |
| parent | number | Parent/manager user ID | No | 
