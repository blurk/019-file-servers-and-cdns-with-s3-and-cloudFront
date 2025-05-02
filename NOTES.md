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

## A CDN like CloudFront has two purposes (as far as the context of this course is concerned):

Speed: Users get content from the server closest to them, which is faster than getting it from the origin server.
Security: The origin server is hidden from the public internet, and only the CDN can access it. This is a security measure that can help prevent DDoS attacks and other malicious activity.

## Availability
It's about how often your service is up and running, serving user requests. It's often measured in "nines" - like "three nines" (99.9%) or "five nines" (99.999%).

Users don't like when they log into your web app and stuff isn't loading. They don't like to hear that you're "down for maintenance".

## Reliability

Reliability is about how well your system works when it's up. For example, maybe your server is responding to HTTP requests, but it's returning erroneous data because some dependency is down. That's not reliable.

The reliability of S3 is very high out of the box.

## Durability
It's about how well your data survives in the event of an outage. For example, let's say you're running your own single server:

- What happens if the intern accidentally rm -rfs the user_pics directory?
- What happens if the server's hard drive fails?
- What happens if the data center it's in catches fire?
=> These are all durability questions. Durability is primarily about backups and redundancy. In the case of S3, it automatically replicates your data across multiple servers. If one goes down, the backups are there.

According to these docs: S3's standard storage provides 99.999999999% durability and 99.99% availability of objects over a given year.