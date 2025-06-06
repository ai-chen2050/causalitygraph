# Social Actions Causality Keys (CIP 06)

## 1. Description

CIP 06 defines a set of generic social interaction keys that can be reused across all subspaces and applications, such as OpenResearch, ModelGraph, and future platforms. These actions include like, collect, share, comment, tag, follow, unfollow, and question. By abstracting these actions, the protocol ensures maximum reusability and extensibility for any collaborative or social scenario.

## 2. Key Features

- **Universal Social Actions**: Support for common social interactions like likes, comments, and shares across all subspaces
- **Flexible Object Types**: Ability to interact with any type of object (papers, posts, comments, etc.)
- **Rich Interaction Context**: Support for threaded discussions, mentions, and replies
- **Chat Functionality**: Built-in support for chat rooms and direct messaging
- **Social Graph**: Track relationships between users through follow/unfollow actions

## 3. Social Actions Subspace

The social actions platform extends the basic Causality Key system with new kinds of events specific to social interactions:

### 3.1 Subspace Creation Event (Kind 30100)

Creating a social subspace with specialized operation types:

```json
{
  "id": "<32 bytes lowercase hex-encoded sha256 hash of the serialized event data>",
  "pubkey": "<32 bytes lowercase hex-encoded ETH address of the event creator>",
  "created_at": "<Unix timestamp in seconds>",
  "kind": 30100,
  "tags": [
    ["d", "subspace_create"],
    ["sid", "0xSOC"],
    ["subspace_name", "social_actions"],
    ["category", "social"],
    ["ops", "like=30600,collect=30601,share=30602,comment=30603,tag=30604,follow=30605,unfollow=30606,question=30607,room=30608,message=30609"],
    ["rules", "energy>100"]
  ],
  "content": "{\"desc\":\"Universal social interaction space for all subspaces\", \"img_url\": \"http://image_addr.png\"}",
  "sig": "<ETH signature>"
}
```

### 3.2 Social Actions Event Types

| Kind Value | Event Name     | Purpose/Scope                  | Key Tags Structure |
|------------|---------------|--------------------------------|--------------------|
| 30600      | Like          | Like any object (paper, post, etc.) | ["auth", "d":"social", "op":"like", "sid", "object_id", "user_id"] |
| 30601      | Collect       | Collect/favorite any object    | ["auth", "d":"social", "op":"collect", "sid", "object_id", "user_id"] |
| 30602      | Share         | Share any object to a platform | ["auth", "d":"social", "op":"share", "sid", "object_id", "user_id", "platform", "clicks"] |
| 30603      | Comment       | Comment on any object          | ["auth", "d":"social", "op":"comment", "sid", "object_id", "user_id", "parent", "content"] |
| 30604      | Tag           | Tag/label any object           | ["auth", "d":"social", "op":"tag", "sid", "object_id", "tag"] |
| 30605      | Follow        | Follow any user or entity      | ["auth", "d":"social", "op":"follow", "sid", "user_id", "target_id"] |
| 30606      | Unfollow      | Unfollow any user or entity    | ["auth", "d":"social", "op":"unfollow", "sid", "user_id", "target_id"] |
| 30607      | Question      | Ask a question about any object| ["auth", "d":"social", "op":"question", "sid", "object_id", "user_id", "content", "quality"] |
| 30608      | Room          | Represents a chat room         | ["auth", "d":"social", "op":"room", "sid", "name", "description", "members"] |
| 30609      | MessageInRoom | Represents a chat message in a room | ["auth", "d":"social", "op":"message", "sid", "room_id", "content", "reply_to", "mentions"] |

## 4. Operation Event Details

### 4.1 Like Event (Kind 30600)

Used to like any object in the system.

```json
{
  "id": "<32 bytes lowercase hex-encoded sha256 hash of the serialized event data>",
  "pubkey": "<32 bytes lowercase hex-encoded ETH address of the event creator>",
  "created_at": "<Unix timestamp in seconds>",
  "kind": 30600,
  "tags": [
    ["auth", "action=1", "key=30600", "exp=500000"],
    ["d", "social"],
    ["op", "like"],
    ["sid", "0xOR"],
    ["object_id", "<paper_id>"],
    ["user_id", "<user_address>"]
  ],
  "content": "",
  "sig": "<ETH signature>"
}
```

### 4.2 Collect Event (Kind 30601)

Used to collect/favorite any object.

```json
{
  "id": "<32 bytes lowercase hex-encoded sha256 hash of the serialized event data>",
  "pubkey": "<32 bytes lowercase hex-encoded ETH address of the event creator>",
  "created_at": "<Unix timestamp in seconds>",
  "kind": 30601,
  "tags": [
    ["auth", "action=1", "key=30601", "exp=500000"],
    ["d", "social"],
    ["op", "collect"],
    ["sid", "0xOR"],
    ["object_id", "<paper_id>"],
    ["user_id", "<user_address>"]
  ],
  "content": "",
  "sig": "<ETH signature>"
}
```

### 4.3 Share Event (Kind 30602)

Used to share any object to external platforms.

```json
{
  "id": "<32 bytes lowercase hex-encoded sha256 hash of the serialized event data>",
  "pubkey": "<32 bytes lowercase hex-encoded ETH address of the event creator>",
  "created_at": "<Unix timestamp in seconds>",
  "kind": 30602,
  "tags": [
    ["auth", "action=2", "key=30602", "exp=500000"],
    ["d", "social"],
    ["op", "share"],
    ["sid", "0xOR"],
    ["object_id", "<paper_id>"],
    ["user_id", "<user_address>"],
    ["platform", "twitter"],
    ["clicks", "0"]
  ],
  "content": "",
  "sig": "<ETH signature>"
}
```

### 4.4 Comment Event (Kind 30603)

Used to comment on any object.

```json
{
  "id": "<32 bytes lowercase hex-encoded sha256 hash of the serialized event data>",
  "pubkey": "<32 bytes lowercase hex-encoded ETH address of the event creator>",
  "created_at": "<Unix timestamp in seconds>",
  "kind": 30603,
  "tags": [
    ["auth", "action=2", "key=30603", "exp=500000"],
    ["d", "social"],
    ["op", "comment"],
    ["sid", "0xOR"],
    ["object_id", "<paper_id>"],
    ["user_id", "<user_address>"],
    ["parent", "<parent_comment_id>"],
    ["content", "This is a great finding!"]
  ],
  "content": "",
  "sig": "<ETH signature>"
}
```

### 4.5 Tag Event (Kind 30604)

Used to tag/label any object.

```json
{
  "id": "<32 bytes lowercase hex-encoded sha256 hash of the serialized event data>",
  "pubkey": "<32 bytes lowercase hex-encoded ETH address of the event creator>",
  "created_at": "<Unix timestamp in seconds>",
  "kind": 30604,
  "tags": [
    ["auth", "action=2", "key=30604", "exp=500000"],
    ["d", "social"],
    ["op", "tag"],
    ["sid", "0xOR"],
    ["object_id", "<paper_id>"],
    ["tag", "quantum-computing"]
  ],
  "content": "",
  "sig": "<ETH signature>"
}
```

### 4.6 Follow Event (Kind 30605)

Used to follow any user or entity.

```json
{
  "id": "<32 bytes lowercase hex-encoded sha256 hash of the serialized event data>",
  "pubkey": "<32 bytes lowercase hex-encoded ETH address of the event creator>",
  "created_at": "<Unix timestamp in seconds>",
  "kind": 30605,
  "tags": [
    ["auth", "action=2", "key=30605", "exp=500000"],
    ["d", "social"],
    ["op", "follow"],
    ["sid", "0xOR"],
    ["user_id", "<user_address>"],
    ["target_id", "<target_address>"]
  ],
  "content": "",
  "sig": "<ETH signature>"
}
```

### 4.7 Unfollow Event (Kind 30606)

Used to unfollow any user or entity.

```json
{
  "id": "<32 bytes lowercase hex-encoded sha256 hash of the serialized event data>",
  "pubkey": "<32 bytes lowercase hex-encoded ETH address of the event creator>",
  "created_at": "<Unix timestamp in seconds>",
  "kind": 30606,
  "tags": [
    ["auth", "action=2", "key=30606", "exp=500000"],
    ["d", "social"],
    ["op", "unfollow"],
    ["sid", "0xOR"],
    ["user_id", "<user_address>"],
    ["target_id", "<target_address>"]
  ],
  "content": "",
  "sig": "<ETH signature>"
}
```

### 4.8 Question Event (Kind 30607)

Used to ask questions about any object.

```json
{
  "id": "<32 bytes lowercase hex-encoded sha256 hash of the serialized event data>",
  "pubkey": "<32 bytes lowercase hex-encoded ETH address of the event creator>",
  "created_at": "<Unix timestamp in seconds>",
  "kind": 30607,
  "tags": [
    ["auth", "action=2", "key=30607", "exp=500000"],
    ["d", "social"],
    ["op", "question"],
    ["sid", "0xOR"],
    ["object_id", "<paper_id>"],
    ["user_id", "<user_address>"],
    ["content", "How does this compare to previous approaches?"],
    ["quality", "high"]
  ],
  "content": "",
  "sig": "<ETH signature>"
}
```

### 4.9 Room Event (Kind 30608)

Used to create and manage chat rooms.

```json
{
  "id": "<32 bytes lowercase hex-encoded sha256 hash of the serialized event data>",
  "pubkey": "<32 bytes lowercase hex-encoded ETH address of the event creator>",
  "created_at": "<Unix timestamp in seconds>",
  "kind": 30608,
  "tags": [
    ["auth", "action=3", "key=30608", "exp=500000"],
    ["d", "social"],
    ["op", "room"],
    ["sid", "0xOR"],
    ["name", "General Chat"],
    ["description", "A place for everyone"],
    ["members", "user1,user2,user3"]
  ],
  "content": "",
  "sig": "<ETH signature>"
}
```

### 4.10 MessageInRoom Event (Kind 30609)

Used to send messages in chat rooms.

```json
{
  "id": "<32 bytes lowercase hex-encoded sha256 hash of the serialized event data>",
  "pubkey": "<32 bytes lowercase hex-encoded ETH address of the event creator>",
  "created_at": "<Unix timestamp in seconds>",
  "kind": 30609,
  "tags": [
    ["auth", "action=2", "key=30609", "exp=500000"],
    ["d", "social"],
    ["op", "message"],
    ["sid", "0xOR"],
    ["room_id", "room123"],
    ["content", "Hello, world!"],
    ["reply_to", "<msg_id>"],
    ["mentions", "user1,user2"]
  ],
  "content": "",
  "sig": "<ETH signature>"
}
```

## 5. Integration with Other Subspaces

Social actions can be integrated with other subspaces to provide social features:

### 5.1 Research Paper Interaction

```json
// Like a research paper
{
  "id": "<hash>",
  "pubkey": "<user_pubkey>",
  "created_at": "<timestamp>",
  "kind": 30600,
  "tags": [
    ["auth", "action=1", "key=30600", "exp=500000"],
    ["d", "social"],
    ["op", "like"],
    ["sid", "0xOR"],
    ["object_id", "<paper_id>"],
    ["user_id", "<user_address>"]
  ],
  "content": "",
  "sig": "<signature>"
}

// Comment on a paper
{
  "id": "<hash>",
  "pubkey": "<user_pubkey>",
  "created_at": "<timestamp>",
  "kind": 30603,
  "tags": [
    ["auth", "action=2", "key=30603", "exp=500000"],
    ["d", "social"],
    ["op", "comment"],
    ["sid", "0xOR"],
    ["object_id", "<paper_id>"],
    ["user_id", "<user_address>"],
    ["content", "This methodology is innovative!"]
  ],
  "content": "",
  "sig": "<signature>"
}
```

### 5.2 Model Discussion

```json
// Create a discussion room for a model
{
  "id": "<hash>",
  "pubkey": "<user_pubkey>",
  "created_at": "<timestamp>",
  "kind": 30608,
  "tags": [
    ["auth", "action=3", "key=30608", "exp=500000"],
    ["d", "social"],
    ["op", "room"],
    ["sid", "0xMG"],
    ["name", "GPT-4 Discussion"],
    ["description", "Discussion about GPT-4 capabilities and applications"],
    ["members", "user1,user2,user3"]
  ],
  "content": "",
  "sig": "<signature>"
}

// Post a message in the model discussion room
{
  "id": "<hash>",
  "pubkey": "<user_pubkey>",
  "created_at": "<timestamp>",
  "kind": 30609,
  "tags": [
    ["auth", "action=2", "key=30609", "exp=500000"],
    ["d", "social"],
    ["op", "message"],
    ["sid", "0xMG"],
    ["room_id", "gpt4_discussion"],
    ["content", "What are the key improvements in GPT-4 compared to GPT-3?"],
    ["mentions", "expert1,expert2"]
  ],
  "content": "",
  "sig": "<signature>"
}
```
