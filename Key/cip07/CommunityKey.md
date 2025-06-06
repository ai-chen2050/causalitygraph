# Community Actions Causality Keys (CIP 07)

## 1. Description

CIP 07 defines a set of generic community and group interaction keys that can be reused across all subspaces and applications, such as OpenResearch, ModelGraph, and future platforms. These actions include creating communities, inviting users, creating channels, and posting messages in channels. This abstraction enables flexible and consistent management of collaborative spaces and communication channels.

## 2. Key Features

- **Community Management**: Create and manage communities with different types and access levels
- **Channel Organization**: Create and manage channels within communities for different topics
- **User Invitation**: Flexible invitation system for adding new members to communities
- **Message Broadcasting**: Post and manage messages in community channels
- **Access Control**: Granular control over community and channel access

## 3. Community Actions Subspace

The community actions platform extends the basic Causality Key system with new kinds of events specific to community management:

### 3.1 Subspace Creation Event (Kind 30100)

Creating a community subspace with specialized operation types:

```json
{
  "id": "<32 bytes lowercase hex-encoded sha256 hash of the serialized event data>",
  "pubkey": "<32 bytes lowercase hex-encoded ETH address of the event creator>",
  "created_at": "<Unix timestamp in seconds>",
  "kind": 30100,
  "tags": [
    ["d", "subspace_create"],
    ["sid", "0xCOM"],
    ["subspace_name", "community_actions"],
    ["category", "community"],
    ["ops", "create=30700,invite=30701,channel_create=30702,channel_message=30703"],
    ["rules", "energy>200"]
  ],
  "content": "{\"desc\":\"Community management space for all subspaces\", \"img_url\": \"http://image_addr.png\"}",
  "sig": "<ETH signature>"
}
```

### 3.2 Community Actions Event Types

| Kind Value | Event Name        | Purpose/Scope                  | Key Tags Structure |
|------------|------------------|-------------------------------|--------------------|
| 30700      | CommunityCreate  | Create a community            | ["auth", "d":"community", "op":"create", "sid", "community_id", "name", "type"] |
| 30701      | CommunityInvite  | Invite user to a community    | ["auth", "d":"community", "op":"invite", "sid", "community_id", "inviter_id", "invitee_id", "method"] |
| 30702      | ChannelCreate    | Create a channel in a community| ["auth", "d":"community", "op":"channel_create", "sid", "community_id", "channel_id", "name", "type"] |
| 30703      | ChannelMessage   | Post a message in a channel   | ["auth", "d":"community", "op":"channel_message", "sid", "channel_id", "user_id", "content", "reply_to"] |

## 4. Operation Event Details

### 4.1 CommunityCreate Event (Kind 30700)

Used to create a new community.

```json
{
  "id": "<32 bytes lowercase hex-encoded sha256 hash of the serialized event data>",
  "pubkey": "<32 bytes lowercase hex-encoded ETH address of the event creator>",
  "created_at": "<Unix timestamp in seconds>",
  "kind": 30700,
  "tags": [
    ["auth", "action=3", "key=30700", "exp=500000"],
    ["d", "community"],
    ["op", "create"],
    ["sid", "0xOR"],
    ["community_id", "openai_researchers"],
    ["name", "OpenAI Researchers"],
    ["type", "public"]
  ],
  "content": "{\"desc\":\"A public community for OpenAI research discussions.\",\"rules\":\"Must be an active researcher\",\"categories\":[\"AI\",\"ML\",\"NLP\"]}",
  "sig": "<ETH signature>"
}
```

### 4.2 CommunityInvite Event (Kind 30701)

Used to invite users to join a community.

```json
{
  "id": "<32 bytes lowercase hex-encoded sha256 hash of the serialized event data>",
  "pubkey": "<32 bytes lowercase hex-encoded ETH address of the event creator>",
  "created_at": "<Unix timestamp in seconds>",
  "kind": 30701,
  "tags": [
    ["auth", "action=2", "key=30701", "exp=500000"],
    ["d", "community"],
    ["op", "invite"],
    ["sid", "0xOR"],
    ["community_id", "openai_researchers"],
    ["inviter_id", "<inviter_address>"],
    ["invitee_id", "<invitee_address>"],
    ["method", "direct"]
  ],
  "content": "{\"message\":\"Welcome to our research community!\",\"role\":\"researcher\",\"permissions\":[\"read\",\"write\",\"invite\"]}",
  "sig": "<ETH signature>"
}
```

### 4.3 ChannelCreate Event (Kind 30702)

Used to create a new channel within a community.

```json
{
  "id": "<32 bytes lowercase hex-encoded sha256 hash of the serialized event data>",
  "pubkey": "<32 bytes lowercase hex-encoded ETH address of the event creator>",
  "created_at": "<Unix timestamp in seconds>",
  "kind": 30702,
  "tags": [
    ["auth", "action=3", "key=30702", "exp=500000"],
    ["d", "community"],
    ["op", "channel_create"],
    ["sid", "0xOR"],
    ["community_id", "openai_researchers"],
    ["channel_id", "general"],
    ["name", "General Discussion"],
    ["type", "text"]
  ],
  "content": "{\"desc\":\"General discussion channel for the OpenAI Researchers community.\",\"rules\":\"Be respectful and constructive\",\"moderators\":[\"<mod1>\",\"<mod2>\"]}",
  "sig": "<ETH signature>"
}
```

### 4.4 ChannelMessage Event (Kind 30703)

Used to post messages in community channels.

```json
{
  "id": "<32 bytes lowercase hex-encoded sha256 hash of the serialized event data>",
  "pubkey": "<32 bytes lowercase hex-encoded ETH address of the event creator>",
  "created_at": "<Unix timestamp in seconds>",
  "kind": 30703,
  "tags": [
    ["auth", "action=2", "key=30703", "exp=500000"],
    ["d", "community"],
    ["op", "channel_message"],
    ["sid", "0xOR"],
    ["channel_id", "general"],
    ["user_id", "<user_address>"],
    ["content", "Hello everyone!"],
    ["reply_to", "<message_id>"]
  ],
  "content": "",
  "sig": "<ETH signature>"
}
```

## 5. Integration with Other Subspaces

Community actions can be integrated with other subspaces to provide community features:

### 5.1 Research Community

```json
// Create a research community
{
  "id": "<hash>",
  "pubkey": "<user_pubkey>",
  "created_at": "<timestamp>",
  "kind": 30700,
  "tags": [
    ["auth", "action=3", "key=30700", "exp=500000"],
    ["d", "community"],
    ["op", "create"],
    ["sid", "0xOR"],
    ["community_id", "quantum_research"],
    ["name", "Quantum Computing Research"],
    ["type", "private"]
  ],
  "content": "{\"desc\":\"Private community for quantum computing researchers\",\"rules\":\"Must have published in quantum computing\"}",
  "sig": "<signature>"
}

// Create a channel for paper discussions
{
  "id": "<hash>",
  "pubkey": "<user_pubkey>",
  "created_at": "<timestamp>",
  "kind": 30702,
  "tags": [
    ["auth", "action=3", "key=30702", "exp=500000"],
    ["d", "community"],
    ["op", "channel_create"],
    ["sid", "0xOR"],
    ["community_id", "quantum_research"],
    ["channel_id", "paper_discussions"],
    ["name", "Paper Discussions"],
    ["type", "text"]
  ],
  "content": "{\"desc\":\"Channel for discussing research papers\",\"rules\":\"Link to papers when discussing\"}",
  "sig": "<signature>"
}
```

### 5.2 Model Development Community

```json
// Create a model development community
{
  "id": "<hash>",
  "pubkey": "<user_pubkey>",
  "created_at": "<timestamp>",
  "kind": 30700,
  "tags": [
    ["auth", "action=3", "key=30700", "exp=500000"],
    ["d", "community"],
    ["op", "create"],
    ["sid", "0xMG"],
    ["community_id", "llm_developers"],
    ["name", "LLM Developers"],
    ["type", "public"]
  ],
  "content": "{\"desc\":\"Community for LLM developers and researchers\",\"rules\":\"Share knowledge and best practices\"}",
  "sig": "<signature>"
}

// Create a channel for model architecture discussions
{
  "id": "<hash>",
  "pubkey": "<user_pubkey>",
  "created_at": "<timestamp>",
  "kind": 30702,
  "tags": [
    ["auth", "action=3", "key=30702", "exp=500000"],
    ["d", "community"],
    ["op", "channel_create"],
    ["sid", "0xMG"],
    ["community_id", "llm_developers"],
    ["channel_id", "architecture"],
    ["name", "Model Architecture"],
    ["type", "text"]
  ],
  "content": "{\"desc\":\"Channel for discussing LLM architecture designs\",\"rules\":\"Include technical details in discussions\"}",
  "sig": "<signature>"
}
```
