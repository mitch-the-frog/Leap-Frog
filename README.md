# The index.js file, containing the commands



const Discord = require('discord.js')
const client = new Discord.Client()

const config = require('./config.json')
const command = require('./command')
const privateMessage = require('./private-message')

client.on('ready', () => {
  console.log('The client is ready!')

  command(client, 'embed', (message) => {
      const logo = 
        'https://media.discordapp.net/attachments/775014382508965918/775875722132717588/unknown.png'

      const embed = new Discord.MessageEmbed()
        .setTitle('Leap Frog - The Bot')
        .setURL('https://www.instagram.com/wolffza1/')
        .setAuthor(message.author.username)
        .setImage(logo)
        .setThumbnail(logo)
        .setFooter('Leap Frog', logo)
        .setColor('#00AAFF')
        .addFields(
        {
            name: 'Leap Frog - The Idea',
            value: 'I was into discord applications, and went about making this one after two failed attempts',
            inline: true,
        },
        {
            name: 'Leap Frog - The Plan',
            value: 'Set up a discord.js with multiple features for discord-wide use',
            inline: true,
        },
        {
            name: 'Leap Frog - Hosting/Updates/Status',
            value: 'I will look into 24/7 hosting, and make a comment if the bot will be offline for a while',
            inline: true,
        }
        )


      message.channel.send(embed)
  })

  command(client, 'createtextchannel', (message) => {
      const name = message.content.replace('/createchannel ', '')

      message.guild.channels.create(name, {
        type: 'text',
      })
      .then((channel) => {  
          const categoryId = '681617249580482613'
          channel.setParent(categoryId)
      })
  })

  command(client, 'createvoicechannel', (message) => {
      const name = message.content.replace('/createvoicechannel ', '')

      message.guild.channels.create(name, {
          type: 'voice'
      })
      .then((channel) => {
        const categoryId = '681617249580482613'
        channel.setParent(categoryId)
        channel.setUserLimit(10)
      })
  })

  privateMessage(client, 'nerd', 'YoCowDude')

  client.users.fetch('641036464662118470').then((user) => {
      user.send('SNUGGLES')
  })

  command(client, ['ping', 'test'], (message) => {
    message.channel.send('Pong!')
  })

  command(client, 'servers', message => {
      client.guilds.cache.forEach(guild => {
          message.channel.send(`${guild.name} has a total of ${guild.memberCount} members`)
      })
  })

  command(client, ['cc', 'clearchannel'], message => {
      if (message.member.hasPermission('Administrator')) {
          message.channel.messages.fetch().then((results) => {
              message.channel.bulkDelete(results)
          })
      }
  })

  command(client, 'status', message => {
      const content = message.content.replace('/status ', '')

      client.user.setPresence({
          activity: {
              name: content,
              type: 0,
          },
      })
  })
})

client.login(config.token)
