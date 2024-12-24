# Developer Guide: Migrating from Instagram Basic Display API to Instagram Graph API

## Introduction
This guide explains how to migrate from the deprecated Instagram Basic Display API to the new Instagram Graph API. The Instagram Graph API provides more robust features for managing and retrieving data for Instagram Professional accounts (Business or Creator).

---

## Key Differences Between APIs

| **Feature**                  | **Basic Display API**                       | **Graph API**                                   |
|------------------------------|---------------------------------------------|-----------------------------------------------|
| Supported Account Types      | Personal and Business accounts             | Professional accounts (Business, Creator)     |
| Access Media Details         | Yes                                         | Yes                                           |
| Fetch Account Metadata       | Limited                                     | Extensive                                     |
| Post Content                 | No                                          | Yes (with `instagram_business_content_publish`) |
| Manage Comments/Messages     | No                                          | Yes                                           |

---

## Prerequisites

1. **Professional Instagram Account**: Convert your Instagram account to a Business or Creator account.
2. **Facebook Page Link**: Link your Instagram account to a Facebook Page.
3. **Facebook App**:
   - Create an business type app at [Meta for Developers](https://developers.facebook.com/).
   - Set up Instagram API integration.
4. **Permissions & Scopes**:
   - Ensure you request the following scopes during user authentication:
     - `instagram_business_basic`
     - `instagram_business_content_publish`
     - `instagram_business_manage_comments`
     - `instagram_business_manage_messages`

---

## Step 1: Obtain Access Tokens

### 1.1 App Dashboard
If you have not implemented Business Login for Instagram, Get an access token via the App Dashboard.

**Steps**:
- In your app dashboard, click Instagram > API setup with Instagram business login in the left side menu.
- Click Generate token next to the Instagram account you want to access.
- Log into Instagram.
- Copy the access token.

Access tokens from the business login flow are short-lived and valid for 1 hour. Access tokens from the App Dashboard are long-lived and are valid for 60 days. 



---

## Step 2: Fetch Instagram Media
Use the Instagram Graph API to fetch media (images, videos, albums) from an Instagram Professional account.

### 2.1 Get Media
**Endpoint**:
```plaintext
GET https://graph.instagram.com/v17.0
```
**Parameters**:
- `instagram_user_id`: Instagram account ID
- `fields`: Specify the fields to retrieve (e.g., `id`, `caption`, `media_url`, `media_type`, `permalink`)
- `access_token`: Access token

**Example Request**:
```bash
curl -X GET \
"https://graph.instagram.com/v17.0/{instagram_user_id}/media?fields=id,caption,media_url,permalink,media_type&access_token={access_token}"
```

**Example Response**:
```json
{
  "data": [
    {
      "id": "17895695668004550",
      "caption": "Beautiful sunset!",
      "media_type": "IMAGE",
      "media_url": "https://scontent.cdninstagram.com/media.jpg",
      "permalink": "https://www.instagram.com/p/XYZ/"
    },
    {
      "id": "17895695668004551",
      "caption": "Beach video",
      "media_type": "VIDEO",
      "media_url": "https://scontent.cdninstagram.com/video.mp4",
      "permalink": "https://www.instagram.com/p/ABC/"
    }
  ]
}
```

---

## Step 3: Secure Your Application
1. **Hide Tokens**:
   - Store access tokens securely in environment variables or a database.
2. **Use Server-Side Code**:
   - Fetch data server-side and pass it to the frontend to avoid exposing tokens.
---
