# Pluto for Channels

This simple Docker image will generate an M3U playlist and EPG optimized for use in [Channels](https://getchannels.com) and expose them over HTTP.

[Channels](https://getchannels.com) supports [custom channels](https://getchannels.com/docs/channels-dvr-server/how-to/custom-channels/) by utilizing streaming sources via M3U playlists.

[Channels](https://getchannels.com) allows for [additional extended metadata tags](https://getchannels.com/docs/channels-dvr-server/how-to/custom-channels/#channels-extensions) in M3U playlists that allow you to give it extra information and art to make the experience better. This project adds those extra tags to make things look great in Channels.

## Set Up

Running the container is easy. Fire up the container as usual. You can set which port it runs on.

    docker run -d --restart unless-stopped --name pluto-for-channels -p 8080:80 gabepb/pluto-for-channels

You can retrieve the playlist and EPG via the status page.

    http://127.0.0.1:8080

### Optionally have multiple feeds generated

By using the `VERSIONS` env var when starting the docker container, you can tell it to create multiple feeds that can be used elsewhere.

Simply provide a comma separated list of words without spaces with the `VERSIONS` env var.

    docker run -d --restart unless-stopped --name pluto-for-channels -p 8080:80 -e VERSIONS=Dad,Bob,Joe gabepb/pluto-for-channels

### Optionally provide a starting channel number

By using the `START` env var when starting the docker container, you can tell it to start channel numbers with this value. Original Pluto channel numbers will be added to this, keeping all of the channels in the same order they are on Pluto.

You should use a starting number greater than 10000, so that the channel numbers will be preserved but not conflict with any other channels you may have.~

For example, channel 345 will be 10345. Channel 2102 will be 12102.

Simpley provide a starting number with the `START` env var.

    docker run -d --restart unless-stopped --name pluto-for-channels -p 8080:80 -e START=80000 gabepb/pluto-for-channels
### Optionally exclude channel categories

By using the `EXCLUDE_CATEGORIES` environment variable when starting the docker container,
you can tell it to exclude channel categories from the .m3u

Provide a comma separated list of categories:

    EXCLUDE_CATEGORIES=Kids,En Español,Music,Gaming + Anime

If running via docker on the command line:

    docker run -d --restart unless-stopped --name pluto-for-channels -p 8080:80 -e "EXCLUDE_CATEGORIES=Kids,En Español,Music,Gaming + Anime" gabepb/pluto-for-channels


## Add Source to Channels

Once you have your Pluto M3U and EPG XML available, you can use it to [custom channels](https://getchannels.com/docs/channels-dvr-server/how-to/custom-channels/) channels in the [Channels](https://getchannels.com) app.

Add a new source in Channels DVR Server and choose `M3U Playlist`. Fill out the form using your new playlist URL.

<img src=".github/1.png" width="400px"/>

Next, set the provider for your new source and choose custom URL.

<img src=".github/2.png" width="300px"/>

Finally, enter your EPG xml url and set it to refresh every 6 hours.

<img src=".github/3.png" width="500px"/>
