---
name: absuite-social
description: >
  Manage the social network in the Alliance Business Suite (ABS) using the `absuite`
  CLI. Covers social profiles, posts, comments, reactions, attachments, groups,
  feeds, conversations, messages, notifications, and follow/unfollow relationships.
  Requires an authenticated CLI session.
---

# Alliance Business Suite — Social Skill

Manage social features through the `absuite` CLI's `social` service.

## Prerequisites

1. **Authenticate first** using `absuite login` (see the `absuite-login` skill).
2. **Discover commands**: `absuite social list-commands`

## Social Profiles

```bash
# List
absuite social list profiles

# Count
absuite social count profiles

# Get a profile
absuite social get profile --SocialProfileId <profile-guid>
```

## Posts

```bash
# List
absuite social list posts --SocialProfileId <profile-guid>

# Count
absuite social count posts --SocialProfileId <profile-guid>

# Get by ID
absuite social get post --SocialPostId <post-guid>

# Create
absuite social create post --SocialProfileId <profile-guid> --SocialPostCreateDto '{
  "Title": "Exciting product launch!",
  "Message": "We are thrilled to announce our new product line.",
  "SocialProfileId": "<profile-guid>"
}'

# Update
absuite social update post --SocialPostId <post-guid> --SocialPostUpdateDto '{...}'

# Delete
absuite social delete post --SocialPostId <post-guid>
```

## Post Comments

```bash
absuite social list post-comments --SocialPostId <post-guid>
absuite social count post-comments --SocialPostId <post-guid>
absuite social get post-comment --SocialPostCommentId <comment-guid>
absuite social create post-comment --SocialPostId <post-guid> --SocialPostCommentCreateDto '{
  "Message": "Great news! Looking forward to it."
}'
absuite social update post-comment --SocialPostCommentId <comment-guid> --SocialPostCommentUpdateDto '{...}'
absuite social delete post-comment --SocialPostCommentId <comment-guid>
```

## Post Reactions

```bash
absuite social list post-reactions --SocialPostId <post-guid>
absuite social count post-reactions --SocialPostId <post-guid>
absuite social get post-reaction --SocialPostReactionId <reaction-guid>
absuite social create post-reaction --SocialPostId <post-guid> --SocialPostReactionCreateDto '{
  "ReactionType": "Like"
}'
absuite social update post-reaction --SocialPostReactionId <reaction-guid> --SocialPostReactionUpdateDto '{...}'
absuite social delete post-reaction --SocialPostReactionId <reaction-guid>
```

## Post Attachments

```bash
absuite social list post-attachments --SocialPostId <post-guid>
absuite social count post-attachments --SocialPostId <post-guid>
absuite social get post-attachment --SocialPostAttachmentId <attachment-guid>
absuite social create post-attachment --SocialPostId <post-guid> --SocialPostAttachmentCreateDto '{...}'
absuite social update post-attachment --SocialPostAttachmentId <attachment-guid> --SocialPostAttachmentUpdateDto '{...}'
absuite social delete post-attachment --SocialPostAttachmentId <attachment-guid>
```

## Groups

```bash
absuite social list groups
absuite social count groups
absuite social get group-by-id --SocialGroupId <group-guid>
absuite social create group --SocialGroupCreateDto '{
  "Name": "Engineering Team",
  "Description": "Internal engineering discussions"
}'
absuite social update group --SocialGroupId <group-guid> --SocialGroupUpdateDto '{...}'
absuite social delete group --SocialGroupId <group-guid>
```

## Social Feed

```bash
# Feed posts
absuite social list feed-posts --SocialProfileId <profile-guid>
absuite social count feed-posts --SocialProfileId <profile-guid>
absuite social get feed-post --FeedPostId <feed-post-guid>
absuite social create feed-post --SocialFeedPostCreateDto '{
  "Title": "Status Update",
  "Message": "Working on something exciting!"
}'
absuite social update feed-post --FeedPostId <feed-post-guid> --SocialFeedPostUpdateDto '{...}'
absuite social delete feed-post --FeedPostId <feed-post-guid>

# Feed notifications
absuite social list feed-notifications
absuite social get notification --NotificationId <notification-guid>
```

## Conversations & Messages

```bash
# Conversations
absuite social list conversations
absuite social count conversations
absuite social create conversation --SocialConversationCreateDto '{...}'

# Messages
absuite social list messages --ConversationId <conversation-guid>
absuite social count messages --ConversationId <conversation-guid>
absuite social create message --ConversationId <conversation-guid> --SocialMessageCreateDto '{
  "Content": "Hello! How are you?"
}'
absuite social update message --MessageId <message-guid> --SocialMessageUpdateDto '{...}'
absuite social delete message --MessageId <message-guid>
```

## Notifications

```bash
absuite social list notifications
absuite social count notifications
absuite social get notification --NotificationId <notification-guid>
```

## Follow / Unfollow

```bash
# Follow a profile
absuite social trace- --SocialProfileId <profile-to-follow-guid>

# Unfollow
absuite social unfollow --SocialProfileId <profile-guid>

# Check if following
absuite social trace-exists --SocialProfileId <profile-guid>

# List follows / followers
absuite social list follows
absuite social count follows
absuite social list followers
absuite social count followers

# List followed/follower profiles
absuite social list followed-profiles
absuite social count followed-profiles
absuite social list follower-profiles
absuite social count follower-profiles
```

## Command Quick Reference

| Action | CLI Command |
|---|---|
| List profiles | `absuite social list profiles` |
| Create post | `absuite social create post --SocialProfileId <guid> --SocialPostCreateDto '{...}'` |
| Comment on post | `absuite social create post-comment --SocialPostId <guid> --SocialPostCommentCreateDto '{...}'` |
| React to post | `absuite social create post-reaction --SocialPostId <guid> --SocialPostReactionCreateDto '{...}'` |
| Follow profile | `absuite social trace- --SocialProfileId <guid>` |
| Unfollow | `absuite social unfollow --SocialProfileId <guid>` |
| List conversations | `absuite social list conversations` |
| Send message | `absuite social create message --ConversationId <guid> --SocialMessageCreateDto '{...}'` |
| List notifications | `absuite social list notifications` |

## Critical Rules

- **Authenticate first.**
- **Social profiles are user-scoped**, not tenant-scoped.
- **Follow command is `trace-`** (the CLI alias for the follow operation).
