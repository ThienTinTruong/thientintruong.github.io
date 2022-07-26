---
layout: post
title: "Analyzing Aces in Tennis"
subtitle: "From a different perspective"
date: 2022-07-25 10:45:13 -0500
background: '/img/posts/another-look-at-tennis-aces/Roger-Federer-Wimbledon-serve-e1562882795916.png'
---
# Introduction

Hello, thanks for dropping by. I'm a life-long fan of tennis and I want to get into data analytics so I thought, why not combine to the two together to create some fun projects for myself? In this first blog post I'd like to take another look at aces stats in tennis. 

# Background 

For those who don't know, in tennis, an ace is a serve that successfully lands in the service box and does not touch the receiving player's racquet. 

In tennis, the three all-time leader of aces served is John Isner, Ivo Karlovic, and Roger Federer. As of this writing, they have 13,813; 13,728; and 11,478 aces, respectively. But I'm not here to talk about them. Their data can easily by found on the [ATP website](https://www.atptour.com/en/stats/aces).

# Objective 
I want to talk about who got aced on the most in the open era. I don't believe this question has been asked before. It's something I've been pondering about for a while so it's about time I dig into the data to find out. 

Caveat: the data set I get is from [Jeff Sackman's repository](https://github.com/JeffSackmann/tennis_atp). I am not sure how accurate or complete it is. When I crunch the data, I find there is some discrepancy between my result and the official ATP website. For example, my result shows [John Isner has 13,936 aces](https://docs.google.com/spreadsheets/d/1YlQAnsyW7_jmyOYFSk1WJ6O6_7u2PxgEgqcNLFfsgFY/edit#gid=262157908) vs the [ATP's 13,814](https://www.atptour.com/en/players/john-isner/i186/player-stats). The margin of error is ~1%. 


# Analysis

So let's get right into it. Here are the stats for the top 10 men for getting aced on count (can't think of a better term),match count,  and average aced on per match. All players must have at least 100 matches.

Just looking at the raw numbers of getting aced on I'm surprised that Federer, Nadal, and Djokovic are all in the top 10. But considering how many matches they've all played the average number of them getting aced on is not bad.

<iframe src="https://public.tableau.com/views/Playersandaces/acedoncount?:display_count=n&:showVizHome=no&:embed=true" height="650" width="800"></iframe>

Here is another look at the top 10 highest average for getting aced on per match.  

<iframe src="https://public.tableau.com/views/Playersandaces/averageacedon?:display_count=n&:showVizHome=no&:embed=true" height="650" width="800"></iframe>

Here we see the biggest servers are not necessarily the worst returners. I've always thought this would be the case but my assumption was wrong. We have the journeyman Denis Kudla leading the pack with getting aced on 10 times per match, followed by the almighty meme lord Tomic. What's really surprising (to me) is Francis Tiafoe being third on the list. 

Interesting to note that these players also have relatively low match counts with the exception of Oliver Rochus. Perhaps if they had played more matches their averages will also drop. One way to test this hypothesis is to see where the average of players in the first set of graphss faired when they had low match counts as well (say ~300 matches). This requires another SQL query or technique that I don't know yet. Once I figured it out I will make an update. 

Oliver Rochus also stands out for another reason, he's the shortest player in the list. But does his height (5'6") have anything to do with him getting aced on so much? As seen in the first set of graphs, big players can also get aced on quite often, such as Tomas Berdych and John Isner, who are 6'5" and 6'10" respectively. 

So let's see how Rochus compares with other short players. Here is a list of tennis players who are below 5'10". 

<iframe src="https://public.tableau.com/views/Playersandaces/acedonheight?:display_count=n&:showVizHome=no&:embed=true" height="650" width="800"></iframe>

The data shows there is no relationship between a player's height and him getting aced on. If anything, David Ferrer (3.8 aces) does even better in this category than Novak Djokovic (4.9 aces), Rafael Nadal (5.4 aces), and Andre Agassi (5.9 aces), who are considered the best returners in history. Again, this is a pleasant surprise as I've always known David Ferrer to be an excellent returner, but this analysis shows he's probably the hardest player to ace. Special shoutout to Michael Chang and Fabrice Santoro as well. What legends!

# Top 10 
So how does the current top 10 do in this category? Here is a look. Some of these players are very young and have low match counts well, but they still perform better than the players in top 10 averages above. Which tells me maybe those guys just aren't that good at returning. So nevermind what I said before about their averages getting lower if they play more matches.

<iframe src="https://public.tableau.com/views/Playersandaces/currenttop10?:display_count=n&:showVizHome=no&:embed=true" height="650" width="800"></iframe>

As you can see, the 2022 top 10 also perform very well against the top 10 from 2012. 

<iframe src="https://public.tableau.com/views/Playersandaces/2012top10?:display_count=n&:showVizHome=no&:embed=true" height="650" width="800"></iframe>

# Conclusion

This analysis dispells a couple misconceptions for me: big servers are poor returners (not really), and short players are easy to easy ace (nope), and the current top 10 perform slightly better compared to their counterparts from 10 years ago.


# Process

Thanks for making it this far. I don't want to include this in the main section to bore you with the technical stuff. But I want to explain my work flow and my thought process so you can see how I got there. First, I download and unzip the entire ATP data set FROM Jeff's repository. Then I load all the ATP matches from 1968 to the present into [DB Browser for SQLite](https://sqlitebrowser.org/). What I got is essentially one big data table for me to play with in SQLite. Each row details an individual match, winner name, loser name, and a host of other categories. It has the following columns: 

> tourney_id,	tourney_name,	surface,	draw_size,	tourney_level,	tourney_date,	match_num,	winner_id,	winner_seed,	winner_entry,	winner_name,	winner_hand,	winner_ht,	winner_ioc,	winner_age,	loser_id,	loser_seed,	loser_entry,	loser_name,	loser_hand,	loser_ht,	loser_ioc,	loser_age,	score,	best_of,	round,	minutes,	w_ace,	w_df,	w_svpt,	w_1stIn,	w_1stWon,	w_2ndWon,	w_SvGms,	w_bpSaved,	w_bpFaced,	l_ace,	l_df,	l_svpt,	l_1stIn,	l_1stWon,	l_2ndWon,	l_SvGms,	l_bpSaved,	l_bpFaced,	winner_rank,	winner_rank_points,	loser_rank,	loser_rank_points

Note that there are two columns for ace stats. One is w_ace (winning player's ace) and the other is l_ace (losing player's ace). To get all the aces for a player, I'd have to add up all the aces in all the matches that he plays. This means adding up all the aces in all the matches he won and lost. Then add up all the matches he plays. Then divide the aces against the players by the matches to get the average. And finally apply the query to players to who have at least 100 matches under their belt and exclude rows with NULL values. The final SQL query would look like this:

```SQL
SELECT player_name, SUM(aces_against), SUM(matches) AS matches,  SUM(aces_against)*1.0/matches AS average_aces_against
FROM
(SELECT winner_name AS player_name, SUM(w_ace) AS aces_against, COUNT(winner_name) AS matches
FROM atp_matches
GROUP BY winner_name
UNION ALL
SELECT loser_name AS player_name, SUM(l_ace) AS aces_against, COUNT(loser_name) AS matches
FROM atp_matches
GROUP BY loser_name)
WHERE matches >= 100
AND player_name IS NOT NULL 
AND matches IS NOT NULL 
AND got_aced IS NOT NULL 
AND aces IS NOT NULL
GROUP BY player_name
```

And this is the [result](https://docs.google.com/spreadsheets/d/1YlQAnsyW7_jmyOYFSk1WJ6O6_7u2PxgEgqcNLFfsgFY/edit#gid=262157908). 

