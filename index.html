require("dotenv").config();
const { Client, GatewayIntentBits, PermissionsBitField } = require("discord.js");
const express = require("express");

const app = express();
app.get("/", (req, res) => res.send("Bot alive"));
app.listen(3000);

const client = new Client({
  intents: [
    GatewayIntentBits.Guilds,
    GatewayIntentBits.GuildMessages,
    GatewayIntentBits.MessageContent,
    GatewayIntentBits.GuildMembers
  ]
});

// track survival levels
const survival = new Map(); // userId -> level

// active game per server
const active = new Map();

const events = [
  {
    name: "🔥 FIRST TO TALK GETS MUTED",
    type: "mute_first"
  },
  {
    name: "☠️ LAST PERSON TO TALK LOSES",
    type: "last_loses"
  },
  {
    name: "🤫 SURVIVE SILENCE FOR 20 SECONDS",
    type: "silence_win"
  },
  {
    name: "⚡ FAST TYPE CHALLENGE (first message wins)",
    type: "first_win"
  }
];

function giveSurvivalRole(member, level) {
  const roleName = `I SURVIVED ${level}`;

  let role = member.guild.roles.cache.find(r => r.name === roleName);

  if (!role) {
    member.guild.roles.create({
      name: roleName,
      reason: "Survival progression"
    }).then(r => member.roles.add(r));
  } else {
    member.roles.add(role);
  }
}

client.on("messageCreate", async (message) => {
  if (message.author.bot) return;

  const guildId = message.guild.id;

  // START GAME ON BOT MENTION
  if (message.mentions.has(client.user)) {
    const event = events[Math.floor(Math.random() * events.length)];

    active.set(guildId, {
      event,
      channel: message.channel.id,
      users: new Set(),
      winner: null
    });

    message.channel.send(`🎮 NEW EVENT STARTED!\n\n**${event.name}**`);
    return;
  }

  const game = active.get(guildId);
  if (!game) return;

  game.users.add(message.author.id);

  const member = message.member;

  // 🔥 FIRST WIN EVENT
  if (game.event.type === "first_win" && !game.winner) {
    game.winner = member.id;

    message.channel.send(`🏆 ${member.user.username} won the event!`);

    upgradeSurvival(member);
    active.delete(guildId);
    return;
  }

  // ☠️ FIRST TO TALK GETS MUTED
  if (game.event.type === "mute_first" && !game.winner) {
    game.winner = member.id;

    try {
      await member.timeout(30 * 60 * 1000, "Lost survival event");
      message.channel.send(`💀 ${member.user.username} got muted for 30 minutes!`);
    } catch (e) {}

    upgradeSurvival(member);
    active.delete(guildId);
    return;
  }

  // ☠️ LAST TO TALK LOSES (simple version)
  if (game.event.type === "last_loses") {
    setTimeout(async () => {
      const users = [...game.users];
      const loserId = users[Math.floor(Math.random() * users.length)];

      const loser = await message.guild.members.fetch(loserId);
      message.channel.send(`☠️ ${loser.user.username} lost the round!`);

      upgradeSurvival(member);

      active.delete(guildId);
    }, 20000);
  }

  // 🤫 SILENCE WIN
  if (game.event.type === "silence_win") {
    setTimeout(async () => {
      const users = [...game.users];
      const winnerId = users[users.length - 1];

      const winner = await message.guild.members.fetch(winnerId);
      message.channel.send(`🏆 ${winner.user.username} survived silence!`);

      upgradeSurvival(winner);

      active.delete(guildId);
    }, 20000);
  }
});

// SURVIVAL SYSTEM
function upgradeSurvival(member) {
  const id = member.id;

  let level = survival.get(id) || 0;
  level++;

  if (level > 3) level = 3;

  survival.set(id, level);

  giveSurvivalRole(member, level);
}

client.login(process.env.TOKEN);
