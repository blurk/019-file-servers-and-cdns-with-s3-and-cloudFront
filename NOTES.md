## Large files
Large files, or "large assets", on the other hand, are giant blobs of data encoded in a specific file format and measured in kilo, mega, or gigabytes. As a simple rule:

- If it makes sense to go into an excel spreadsheet, it probably belongs in a traditional database
- If it would normally be stored on your hard drive as its own file, it probably is a "large file"

## Single Machine
This is a perfectly valid way to build a web application, even in production. That said, there are some trade-offs:

- Scaling: If your app gets popular, you'll need to "scale" your single machine (add more resources like CPU/RAM/Disk space). A single computer can only become so powerful.
- Availability: If your server goes down, your app goes down. To be fair, you can mitigate this with load balancers and multiple servers.
- Durability: If your server crashes, or an intern rm -rf's something, you're in trouble. You might have backups, but let's be honest, you probably don't.
- Cost: Running a server 24/7 means paying 24/7. It can be nice to only pay for what you use.
- Maintenance: You have to manage everything yourself. You'll be responsible for "ops" tasks like backups, monitoring, logging, version upgrades, etc.

## Traditional File Storage
"File storage" is what you're already familiar with:

- Files are stored in a hierarchy of directories
- A file's system-level metadata (like timestamp and permissions) is managed by the file system, not the file itself
- File storage is great for single-machine-use (like your laptop), but it doesn't distribute well across many servers. It's optimized for low-latency access to a small number of files on a single machine.

## Object Storage
Object storage is designed to be more scalable, available, and durable than file storage because it can be easily distributed across many machines:
- Objects are stored in a flat namespace (no directories)
- An object's metadata is stored with the object itself

***Schema architecture matters in a SQL database, and prefix architecture matters in S3. We always want to group objects in a way that makes sense for our case, because often we'll want to operate on a group of objects at once.***

## Approaches to videos
1. **Adaptive streaming**: Standard mp4 files have a single resolution and bitrate. If a user's connection speed is unstable, HLS or MPEG-DASH allows for changing the quality of the stream on the fly. You may have noticed on YouTube or Netflix that your video quality changes based on your connection speed. Dropping to lower resolution is better than endlessly buffering.
2. **Live streaming**: Standard mp4 files are not designed to be updated in real-time. You'd want to use a lower-latency protocol like WebRTC or RTMP for live streaming.

## Private bucket

### A good use case for a public bucket might be:

- Users' profile pictures
- Public certificates of completion (we do this for Boot.dev!)
- Dynamically generated images for social sharing (like the link previews you see on Twitter)

### While a private bucket might contain

- A user's privately uploaded documents
- A user's draft content that they haven't published yet
- The org's video content that's only available to paying customers

## Encryptions
Files in S3 are encrypted at rest ("at rest" just means "while they're sitting in storage on disk") by default. This was not always the case, but it is now! You don't need to do anything, the S3 service takes care of all of that for you. When you access S3 with your credentials, the service decrypts the files for you before handing them over.