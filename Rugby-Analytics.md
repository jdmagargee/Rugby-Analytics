Rugby-Analytics
================
Jack Magargee
8/13/2021

EPA calculation, WP is done later following a similar process

Test Plot to visualize field and understand which metrics represent
length and width

``` r
test_plot<- ggplot() +
  geom_point(data = iona_game_res, aes(x = as.numeric(X), y = as.numeric(Y), color = simplex_play_type)) 
test_plot
```

![](README_figs/README-unnamed-chunk-10-1.png)<!-- -->

EPA by field
position

``` r
Team <- c('Notre Dame','Syracuse','Iona','St. Bonaventure','Michigan','Kutztown')
colors1 <- c('#0C2340','#D44500','#6F2C3F','#54261A','#00274C','#77253A')
colors2 <- c('#AE9142','#3E3D3C','#F2A900','#FDB726','#FFDB05','#7E6F42')
crests <- c('https://upload.wikimedia.org/wikipedia/en/7/71/NDRFC_crest.png',
                'https://www.urugby.com/sites/default/files/styles/front-normal-sponsor_300wide/public/syracuse-university.png?itok=_Y6MmfN0',
                'https://icgaels.com/images/logos/site/site.png', #No crest
                'https://s3-us-west-2.amazonaws.com/theathletic-team-logos/team-logo-326-300x300.png', #Their new logo isn't a png even though it's sooooo much better
                'https://lirp-cdn.multiscreensite.com/5fcdb223/dms3rep/multi/opt/rugby-crest-640w.jpg', #No PNG
                'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSD0FxVZT0agtaXu72F9zLpnkJHKbjuozHFLA&usqp=CAU')
TX <- c(1,2,3,4,5,6)
TY <- c(1,2,3,4,5,6)
logos_df <- data.frame(Team,colors1,colors2,crests,TX,TY)

#for later
ND_blue <- logos_df$colors1[1]
Syracuse_orange <- logos_df$colors1[2]
Iona_red <- logos_df$colors[3]
stbonaventure_brown <- logos_df$colors1[4]
michigan_maize <- logos_df$colors2[5]
kutztown_red <- logos_df$colors1[6]
```

Logo viz for use later

![](README_figs/README-unnamed-chunk-16-1.png)<!-- -->

EPA by field posiition (X represents length)

``` r
g_3 <- ggplot(iona_game_res,
              aes(x = X, color = simplex_play_type)) + 
  geom_point(aes(y = EPA), size = 2) +
  theme_bw() 

g_3
```

![](README_figs/README-unnamed-chunk-18-1.png)<!-- -->

EPA by time

``` r
g_4 <- ggplot(iona_game_res,
              aes(x = TimeEnd, color = simplex_play_type)) + 
  geom_point(aes(y = EPA), size = 2) +
  theme_bw() 

g_4
```

![](README_figs/README-unnamed-chunk-19-1.png)<!-- -->

EPA by playtype and field position

``` r
g_10 <- ggplot(iona_game_res)+
  geom_point(aes(x = X, y=EPA, color = Team),size =2)+
  theme_bw()+
  facet_wrap(facets = vars(simplex_play_type))


g_10
```

![](README_figs/README-unnamed-chunk-20-1.png)<!-- -->

Penalty kick chart

``` r
penalty_plot <- ggplot() +
  ggtitle("Penalty Conversions in Notre Dame Matches") +
  annotate("text", x = c(150,530), 
           y = c(600,600), label = "10", hjust = 0, vjust = -0.2,angle =90, color = "black") + 
  annotate("text", x = c(530,150), 
           y = c(780,780), label = "22", hjust = 0, vjust = -0.2,angle =90, color = "black") + 
  annotate("text", x = c(150,530), 
           y = c(1000,1000), label = "G", hjust = 0, vjust = -0.2,angle =90, color = "black") + 
  annotate("text", x = c(150,530), 
           y = c(500,500), label = "50", hjust = 0, vjust = -0.2,angle =90, color = "black") + 
  annotate("segment", x = 0, 
           y = c(600,780,1000,500), 
           xend =  680, 
           yend = c(600,780,1000,500), color = "black") + 
  geom_point(data = penalty_res, aes(x = xmax-as.numeric(Y), y = as.numeric(X), shape = PlayType, color = PlayType, size = 5), alpha = 0.7) +
  scale_shape_manual(values = c(17,16)) +
  scale_colour_manual(values=c("black","red")) +
  theme(panel.background = element_rect(fill = 'light green', colour = 'black')) +
  ylim(500, ymax) +
  coord_fixed() +
  theme(panel.grid.major = element_blank(), panel.grid.minor = element_blank()) +
  labs(x = "MidField", y = "Sideline")  # Set labels 

penalty_plot
```

![](README_figs/README-unnamed-chunk-22-1.png)<!-- -->

Overall kicking chart

``` r
kicking_res <- ggplot() +
  ggtitle("Opponents vs ND Kicking") +
  annotate("text", x = c(150,530), 
           y = c(600,600), label = "10", hjust = 0, vjust = -0.2,angle =90, color = "black") + 
  annotate("text", x = c(530,150), 
           y = c(780,780), label = "22", hjust = 0, vjust = -0.2,angle =90, color = "black") + 
  annotate("text", x = c(150,530), 
           y = c(1000,1000), label = "G", hjust = 0, vjust = -0.2,angle =90, color = "black") + 
  annotate("text", x = c(150,530), 
           y = c(500,500), label = "50", hjust = 0, vjust = -0.2,angle =90, color = "black") + 
  annotate("segment", x = 0, 
           y = c(600,780,1000,500), 
           xend =  680, 
           yend = c(600,780,1000,500), color = "black") + 
  geom_point(data = kicking_res, aes(x = xmax-as.numeric(Y), y = as.numeric(X), shape = PlayType, color = PlayType, size = 5), alpha = 0.5) +
  scale_shape_manual(values = c(17,16,15,16)) +
  scale_colour_manual(values=c("black","red","blue","pink")) +
  theme(panel.background = element_rect(fill = 'light green', colour = 'black')) +
  ylim(500, ymax) +
  facet_wrap(facets = vars(ND_Possession))+
  coord_fixed() +
  theme(panel.grid.major = element_blank(), panel.grid.minor = element_blank()) +
  labs(x = "MidField", y = "Sideline")  # Set labels 

#Go into zoom to see this onen well
kicking_res
```

![](README_figs/README-unnamed-chunk-24-1.png)<!-- -->

Geospatial tracking plot, not great now but the goal is to have a frame
up for when better tracking data is available

``` r
g_9track <- ggplot() +
  scale_size_manual(values = c(6, 6), guide = FALSE) + 
  scale_shape_manual(values = c(21, 21), guide = FALSE) +
  scale_fill_manual(values = c("#6F2C3F", "#0c2340"), guide = FALSE) + 
  scale_colour_manual(values = c("#F2A900", "#d39F10"), guide = FALSE) +
  annotate("text", x = c(150,530,150,530), 
           y = c(400,600,600,400), label = "10", hjust = 0, vjust = -0.2,angle =90, color = "black") + 
  annotate("text", x = c(150,530,150,530), 
           y = c(220,220,780,780), label = "22", hjust = 0, vjust = -0.2,angle =90, color = "black") + 
  annotate("text", x = c(150,530,150,530), 
           y = c(0,0,1000,1000), label = "G", hjust = 0, vjust = -0.2,angle =90, color = "black") + 
  annotate("text", x = c(150,530), 
           y = c(500,500), label = "50", hjust = 0, vjust = -0.2,angle =90, color = "black") + 
  annotate("segment", x = 0, 
           y = c(400,600,220,780,0,1000,500), 
           xend =  680, 
           yend = c(400,600,220,780,0,1000,500), color = "black") + 
  geom_point(data = iona_game_res, aes(x = xmax-as.numeric(Y), y = as.numeric(X), shape = Team, fill = Team, size = Team,
                                       color = Team), alpha = 0.7) +
 # labs(title = "ND: {iona_game_res$NDScore}") +
  transition_time(iona_game_res$TimeBegin)+
  theme(panel.background = element_rect(fill = 'light green', colour = 'black')) +
  ylim(ymin, ymax) +
  coord_fixed() +
  theme(panel.grid.major = element_blank(), panel.grid.minor = element_blank()) +
  labs(x = "TryLine", y = "Sideline")  # Set labels 
#  title = paste("ND v Iona Timelaps - {iona_game_res$NDScore}",)

g_9track
```

![](README_figs/README-unnamed-chunk-25-1.gif)<!-- -->

Series of win probability plots, all follow this framework

``` r
cuse_wp <- ggplot(syracuse_game_res) + 
  geom_smooth(aes(x = game_seconds_remaining,y = NDwp), size = 2, color = ND_blue) +
  geom_smooth(aes(x = game_seconds_remaining,y = Oppwp), size = 2, color = Syracuse_orange) + 
  geom_vline(xintercept = 5000, linetype = 'dashed', color = 'black') +
#  annotate("text", size = 8, x = 5000, y = 1, label = "Gaels", color = '#6F2C3F') +
#  annotate("text", size = 8, x = 3000, y = .25, label = "Fighting Irish", color = '#0C2340') +
#  transition_time(game_seconds_remaining)+    #I want to explore transition time here
  labs( x = "Time Passed (seconds)", 
        y = "Win Probability",
        title = "Notre Dame v Syracuse",
        subtitle = "In Syracuse, NY") + 
  theme(panel.grid.major = element_blank(), panel.grid.minor = element_blank()) +
  theme_bw() 

cuse_wp
```

![](README_figs/README-unnamed-chunk-26-1.png)<!-- -->

The thought is that with more games uploaded in the future, these will
become more
accurate

![](README_figs/README-unnamed-chunk-27-1.png)<!-- -->![](README_figs/README-unnamed-chunk-27-2.png)<!-- -->![](README_figs/README-unnamed-chunk-27-3.png)<!-- -->

EPA Grid

``` r
grid_g <- ggplot(data = grid_vals) +
  annotate("text", x = c(150,530,150,530), 
           y = c(400,600,600,400), label = "10", hjust = 0, vjust = -0.2,angle =90, color = "black") + 
  annotate("text", x = c(150,530,150,530), 
           y = c(220,220,780,780), label = "22", hjust = 0, vjust = -0.2,angle =90, color = "black") + 
  annotate("text", x = c(150,530,150,530), 
           y = c(0,0,1000,1000), label = "G", hjust = 0, vjust = -0.2,angle =90, color = "black") + 
  annotate("text", x = c(150,530), 
           y = c(500,500), label = "50", hjust = 0, vjust = -0.2,angle =90, color = "black") + 
  annotate("segment", x = 0, 
           y = c(400,600,220,780,0,1000,500), 
           xend =  680, 
           yend = c(400,600,220,780,0,1000,500), color = "black") +
  theme(panel.background = element_rect(fill = 'light green', colour = 'black')) +
  theme(panel.grid.major = element_blank(), panel.grid.minor = element_blank()) +
  geom_rect(aes(xmin = xmin,  # Set minimum x value
                xmax = xmax ,  # Set maximum x value
                ymin =  ymin, # Set minimum y value
                ymax = ymax,  # Set maximum y value
                fill = avgEPA), # Set fill of each rectangle
            alpha = 0.9) + # Set transperancy
  scale_fill_gradient(low = "blue", high = "red") # Scale fill gradient 

grid_g
```

![](README_figs/README-unnamed-chunk-29-1.png)<!-- -->

EPA grid by position group

``` r
ggarrange(grid_g,grid_forward,grid_back,
         labels = c("Total","Forwards","Backs"),
         ncol = 3, nrow = 1,heights = c(1,1,1), widths = c(2,2,2))
```

![](README_figs/README-unnamed-chunk-31-1.png)<!-- -->
