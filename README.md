# pygame-test_game
Pygame game
from datetime import datetime, timedelta
import pygame
from pygame.locals import *
import math
import random
pygame.init()

#window
win = pygame.display.set_mode((1000, 600))# remember to change screen_height and screen_width if you change these
pygame.display.set_caption("Gwen's Game")
screen_height = 600
screen_width = 1000
background = pygame.image.load("True_background_two.jpg")

bgx_menu = 0 # controls the background during the menu
bgy_menu = 0

bgx = 0
bgx_mid = screen_width
bgy = 0
bgy_mid = screen_height
clock = pygame.time.Clock()
menu_clock = pygame.time.Clock()
tile_x = 200
tile_y = 100
mouse_click = False
music = pygame.mixer.music.load('pygame_song_1.mp3')
pygame.mixer.music.play(-1)

hit_enemy_sound = pygame.mixer.Sound('hit_enemy_sound.wav')
player_hit_sound = pygame.mixer.Sound('player_hit_sound.wav')
shoot_sound = pygame.mixer.Sound('shoot_sound.wav')
#**************************MAIN_MENU*****************************************************
main_menu = True
while main_menu is True:    
    win.fill(0)
    
    rel_x_menu = bgx_menu % screen_width
    win.blit(background, (rel_x_menu - screen_width, bgy_menu))
    if rel_x_menu < screen_width:
        win.blit(background, (rel_x_menu, 0))
    bgx_menu -= 0.3
        
    mx, my = pygame.mouse.get_pos()

    menu_text = pygame.font.SysFont('free', 64)
    title_text = pygame.font.SysFont('free', 96)

    title_message = title_text.render("GWEN'S B-DAY ADVENTURE", True, (135,206,255)) # (135,206,255) Baby-blue, (65,105,225) Darker-blue GWEN'S B-DAY ADVENTURE
    win.blit(title_message, (50, 200))
    
    start_message = menu_text.render("START", True, (255,255,255))
    start_message_red = menu_text.render("START", True, (255,0,0))
    win.blit(start_message, (275, 360))

    quit_message = menu_text.render("QUIT", True, (255,255,255))
    quit_message_red = menu_text.render("QUIT", True, (255,0,0))
    win.blit(quit_message, (565, 360))
    
    start_button = pygame.Rect(260, 355, 175, 50)
    #pygame.draw.rect(win, (255, 255, 255), start_button, 2) # draws start button
    
    exit_button = pygame.Rect(550, 355, 135, 50)
    #pygame.draw.rect(win, (255,255,255), exit_button, 2)

    if start_button.collidepoint((mx, my)):
        win.blit(start_message_red, (275, 360))
        if mouse_click is True:
            main_menu = False

    if exit_button.collidepoint((mx, my)):
        win.blit(quit_message_red, (565, 360))
        if mouse_click is True:
            pygame.quit()


    for event in pygame.event.get():
        if event.type == QUIT:
            pygame.quit()

        if event.type == MOUSEBUTTONDOWN: # if mouse is clicked
            if event.button == 1: # left mouse button
                mouse_click = True

    #menu_clock.tick(72)
    pygame.display.update()
#****************************************************************************************
#sprites and tiles
ramen_sprite = pygame.image.load('True_ramen.png')
bridge_tile = pygame.image.load('Bridge_tile.png')
player_sprite_right = pygame.image.load('HGB_char.png')
player_sprite_left = pygame.image.load('HGB_char_left.png')
player_sprite_hit_right = pygame.image.load('HGB_char_hit.png')
player_sprite_hit_left = pygame.image.load('HGB_char_hit_left.png')
enemy_sprite_right = pygame.image.load('True_teacher.png')
full_health = pygame.image.load('True_full_health.png')
empty_health = pygame.image.load('True_empty_health.png')
plat_tile = pygame.image.load('Platform_tile.png')
build_border = pygame.image.load('build_border.png')
boss_sprite = pygame.image.load('True_boss_small.png')
boss_sprite_shoot = pygame.image.load('True_boss_small2.png')
boss_sprite_bullet = pygame.image.load('True_boss_shoot.png')
cat_1 = pygame.image.load("True_cat_1.png")
cat_2 = pygame.image.load("True_cat_2.png")

boss_death = [pygame.image.load('True_boss_small_death1.png'), pygame.image.load('True_boss_small_death2.png'), pygame.image.load('True_boss_small_death3.png'), pygame.image.load('True_boss_small_death4.png'), pygame.image.load('True_boss_small_death5.png'), pygame.image.load('True_boss_small_death6.png')]

enemy_right = [pygame.image.load('True_teacher1.png'), pygame.image.load('True_teacher2.png'), pygame.image.load('True_teacher3.png'), pygame.image.load('True_teacher4.png'), pygame.image.load('True_teacher5.png'), pygame.image.load('True_teacher6.png'), pygame.image.load('True_teacher7.png'), pygame.image.load('True_teacher8.png'), pygame.image.load('True_teacher9.png')]
enemy_left = [pygame.image.load('True_teacher_left1.png'), pygame.image.load('True_teacher_left2.png'), pygame.image.load('True_teacher_left3.png'), pygame.image.load('True_teacher_left4.png'), pygame.image.load('True_teacher_left5.png'), pygame.image.load('True_teacher_left6.png'), pygame.image.load('True_teacher_left7.png'), pygame.image.load('True_teacher_left8.png'), pygame.image.load('True_teacher_left9.png') ]

player_right = [pygame.image.load('HGB_char1.png'), pygame.image.load('HGB_char2.png'), pygame.image.load('HGB_char3.png'), pygame.image.load('HGB_char4.png'), pygame.image.load('HGB_char5.png'), pygame.image.load('HGB_char6.png'), pygame.image.load('HGB_char7.png'), pygame.image.load('HGB_char8.png'), pygame.image.load('HGB_char9.png')]
player_left = [pygame.image.load('HGB_char_left1.png'), pygame.image.load('HGB_char_left2.png'), pygame.image.load('HGB_char_left3.png'), pygame.image.load('HGB_char_left4.png'), pygame.image.load('HGB_char_left5.png'), pygame.image.load('HGB_char_left6.png'), pygame.image.load('HGB_char_left7.png'), pygame.image.load('HGB_char_left8.png'), pygame.image.load('HGB_char_left9.png')]

bug_right = [pygame.image.load('True_bug1.png'), pygame.image.load('True_bug2.png')]
bug_left = [pygame.image.load('True_bug_left 1.png'), pygame.image.load('True_bug_left 2.png')]

#game_map
#5 wide and six tall makes one screen instance
game_map = [['0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','5','4','5','4','4','4','4','4','4','4','5'],
            ['0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','4','5','4','4','5','5','4','5','5','4','5'],
            ['0','0','0','0','0','0','0','0','2','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','5','4','5','4','5','4','5','4','4','5','4'],
            ['0','0','0','0','0','0','0','2','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','4','5','5','5','4','5','4','5','5','5','5'],
            ['0','0','0','0','0','0','2','0','2','0','0','0','0','2','0','0','0','0','0','2','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','0','4','5','4','4','5','5','4','5','4','4'],
            ['1','1','1','1','1','1','1','1','3','3','1','1','1','1','1','1','1','1','1','3','1','3','1','1','1','1','1','3','3','1','1','1','3','3','3','3','3','1','1','1','1','1','1','3','3','1','1','1','1','3','1','1','1','1','1','1','1','1','1']]
#player
#***************************************************************************
class player(object):
    def __init__(self, x, y, width, height):
        self.x = x
        self.y = y
        self.width = width
        self.height = height
        self.vel = 5
        self.y_vel = 0
        self.isJump = False
        self.jump_vel = 16
        self.left = False
        self.right = False
        self.walkCount = 0
        self.standing = True
        self.hearts = 3
        self.max_hearts = 10
        self.damage_timer = 0
        self.isHit = False
        self.score_counter = 0

        super().__init__()
        self.invincible_until = datetime.now() # defines the invisible timer as being equal to now(so the plyer can take damage initially)

        super().__init__()
        self.cannot_get_health_until = datetime.now()

    def can_take_damage(self):
        if datetime.now() < self.invincible_until: # want the now time to be less than the invincible timer for the player to take be invinsible. othereise, return true.
            return False

        return True

    def can_get_health(self):
        if datetime.now() < self.cannot_get_health_until:
            return False

        return True

    def draw(self, win):
        if self.walkCount + 1 >= 72:
            self.walkCount = 0
        
        if not (self.standing):
            if self.right is True:
                win.blit(player_right[self.walkCount // 8], (self.x, self.y))
                self.walkCount += 1
            
            elif self.left is True:
                win.blit(player_left[self.walkCount // 8],(self.x, self.y))
                self.walkCount += 1
        else:
            if self.right is True:
                win.blit(player_sprite_right, (self.x, self.y))
            else:
                win.blit(player_sprite_left, (self.x, self.y))

        self.hitbox = pygame.Rect(self.x + 7, self.y , self.width - 14, self.height)
        #pygame.draw.rect(win, (255,0,0), self.hitbox, 2)

    def get_health(self):
        if self.can_get_health():
            if self.hearts <= self.max_hearts:
                self.hearts += 1
                self.cannot_get_health_until = datetime.now() + timedelta(seconds = 2)

    def hit(self):
        if self.can_take_damage():
            if self.hearts > 0:
                player_hit_sound.play()
                self.hearts -= 1
                self.invincible_until = datetime.now() + timedelta(seconds = 2) # invincible timer is equal to now() (0 second) + the timer. (1 second)

                if self.right is True:
                    pygame.time.delay(200)

                elif self.left is True:
                    pygame.time.delay(200)

        if self.hearts == 0:
            self.death()
            
    def full_hearts(self, win):
        for heart in range(self.hearts):
            win.blit(full_health, (heart * 50 + 123, 10))

    def death(self):
        self.x = 350
        self.y = 100
        self.walkCount = 0
        self.score_counter = 0 
        self.hearts = 3
        win.blit(death_message.death, (death_message.x - 100, death_message.y))
        pygame.display.update()
        pygame.time.delay(1000)
        chem.visible = True
        chem_2.visible = True
        chem_3.visible = True
        chem_4.visible = True
        chem_5.visible = True
        chem_6.visible = True
        chem_7.visible = True
        chem_8.visible = True

        bug_1.visible = True
        bug_2.visible = True
        bug_3.visible = True
        bug_4.visible = True
        bug_5.visible = True

        heart_1.visible = True
        heart_2.visible = True
        heart_3.visible = True
        heart_4.visible = True

        boss_1.health = 50
        boss_1.x = 11700
#**************************************************************************

# teacher
#**************************************************************************
class teacher(object):
    def __init__(self, x, y, width, height, end):
        self.x = x
        self.y = y
        self.width = width
        self.height = height
        self.end = end
        self.walk_count = 0
        self.vel = 3
        self.path = [self.x, self.end]
        self.visible = True

    def draw(self, win):
        self.move() # teacher must be moved before drawn every time
        if self.visible is True:
            if self.walk_count + 1 >= 72:
                self.walk_count = 0

            if self.vel > 0:
                win.blit(enemy_right[self.walk_count// 8], (self.x + bgx, self.y + bgy))
                self.walk_count += 1
            else:
                win.blit(enemy_left[self.walk_count // 8], (self.x + bgx, self.y + bgy))
                self.walk_count += 1

            self.hitbox = pygame.Rect(self.x + bgx + 7, self.y + bgy, self.width - 14, self.height)
            #pygame.draw.rect(win, (255,0,0), self.hitbox, 2)

    def move(self):
        if self.vel > 0: # if the character is moving. (This is creating the walk path for the character)
            if self.x + self.vel < self.path[1]: # if the teachers x pos + velocity is less theb the end of the path, the teacher can move
                self.x  += self.vel
            else:
                self.vel = self.vel * -1 # changes direction 
                self.walk_count = 0
        else:
            if self.x - self.vel > self.path[0]:
                self.x += self.vel
            else:
                self.vel = self.vel * -1 # changes direction 
                self.walk_count = 0

    def hit(self):
        print('hit')
        noodles.pop(noodles.index(noodle))
        shoot_sound.play()
    
#*************************************************************************

#****************************BUG******************************************
class bug(object):
    def __init__(self, x, y, width, height, end):
        self.x = x
        self.y = y
        self.width = width
        self.height = height
        self.end = end
        self.walk_count = 0
        self.vel = 3
        self.path = [self.x, self.end]
        self.visible = True
        
    def draw(self, win):
        self.move()
        if self.visible is True:
            if self.walk_count + 1 >= 72:
                self.walk_count = 0

            if self.vel > 0:
                win.blit(bug_right[self.walk_count // 36], (self.x + bgx, self.y + bgy))
                self.walk_count += 1
            else:
                win.blit(bug_left[self.walk_count // 36], (self.x + bgx, self.y + bgy))
                self.walk_count += 1

            self.hitbox = pygame.Rect(self.x + bgx, self.y, self.width, self.height)
            #pygame.draw.rect(win, (255,0,0), self.hitbox, 2)

    def move(self):
        if self.vel > 0:
            if self.x + self.vel < self.path[1]:
                self.x  += self.vel
            else:
                self.vel = self.vel * -1 
                self.walk_count = 0
        else:
            if self.x - self.vel > self.path[0]:
                self.x += self.vel
            else:
                self.vel = self.vel * -1
                self.walk_count = 0

    def hit(self):
        print('hit')
        noodles.pop(noodles.index(noodle))
        shoot_sound.play()      
#*************************************************************************

#**********************************BOSS***********************************
class boss(object):
    def __init__(self, x, y, width, height):
        self.x = x
        self.y = y
        self.width = width
        self.height = height
        self.health = 50
        self.visible = True
        self.start = False
        self.death_count = 0
        self.death_animation = 3
        self.end_death = False
        self.end_game = False

    def draw(self, win):
        if self.visible is True:
            win.blit(boss_sprite, (self.x + bgx, self.y + bgy))
            self.hitbox = pygame.Rect(self.x + bgx, self.y + bgy, self.width, self.height)
            #pygame.draw.rect(win, (255, 0, 0), self.hitbox, 2)
            if bgx < -10820:
                self.start = True
            if self.start is True:
                self.x -= 0.3
        else:
            if self.end_death is False:
                win.blit(boss_death[self.death_count // 12], (self.x + bgx, self.y + bgy))
                self.death_count += 1
                if self.death_animation > 0:
                    if self.death_count >= 72 - 1:
                        self.death_count = 0
                        self.death_animation -= 1
                else:
                    self.end_death = True
                    self.end_game = True
                        
    def hit(self):
        try:
            noodles.pop(noodles.index(noodle))
        except ValueError:
            noodles.append(ramen(round(BDG.x), round(BDG.y), 32, 32, facing))
        shoot_sound.play()
        if self.health > 0:
            self.health -= 1

        else:
            self.visible = False

    def health_bar(self, win):
        if self.start is True: # bar width = health * 16
            if self.end_game is False:
                self.bar = pygame.Rect(100, 75, 800, 10)
                self.bar_full = pygame.Rect(100, 75, self.health * 16, 10)
                pygame.draw.rect(win, (255,0,0), self.bar)
                pygame.draw.rect(win, (0,255,0), self.bar_full)
#*************************************************************************

#*****************************BOSS_BULLETS********************************
class boss_bullet(object):
    def __init__(self, x, y, width, height):
        self.x = x
        self.y = y
        self.width = width
        self.height = height
        self.vel = 5
        self.hitbox = pygame.Rect(self.x + bgx, self.y, 28, 31)

    def draw(self, win):
        win.blit(boss_sprite_bullet, (self.x + bgx, self.y))
        self.hitbox = pygame.Rect(self.x + bgx, self.y, 28, 31)
        #pygame.draw.rect(win, (255,0,0), self.hitbox, 2)
#*************************************************************************

#****************************PLAT FORM************************************
class plat(object):
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def draw(self, win):
        win.blit(plat_tile, (self.x + bgx, self.y + 10  + bgy))
        self.hitbox = pygame.Rect(self.x + bgx, self.y + bgy, plat_tile.get_width(), 1)
        #pygame.draw.rect(win, (255,0,0), self.hitbox, 2)
#*************************************************************************

#*****************************projectile**************************************
class ramen(object):
    def __init__(self, x, y, width, height, facing):
        self.x = x
        self.y = y
        self.width = width
        self.height = height
        self.facing = facing
        self.vel = 8

    def draw(self, win):
        win.blit(ramen_sprite,(self.x, self.y))
        self.hitbox = pygame.Rect(self.x + 5, self.y + 7, 32 - 10, 32 - 15)
        #pygame.draw.rect(win, (255, 0, 0), self.hitbox, 2)
#**************************************************************************

#********************************HEARTS************************************
class ground_hearts(object):
    def __init__(self, x, y):
        self.x = x
        self.y = y
        self.visible = True
        
    def draw(self, win):
        if self.visible is True:
            win.blit(full_health,(self.x + bgx, self.y+ bgy))
            self.hitbox = pygame.Rect(self.x + bgx, self.y + bgy, 40, 40)
            #pygame.draw.rect(win, (255, 0, 0), self.hitbox, 2)
#**************************************************************************

# key_lightups
#*******************************************************************************************
class key_indicators(object):
    def __init__ (self, x, y, width, height):
        self.x = x
        self.y = y
        self.width = width
        self.height = height
        self.space = False
        self.right = False
        self.left = False
        self.shoot = False

    def draw_space(self, win):
        pygame.draw.rect(win, (255, 255, 255), (self.x, self.y, self.width, self.height))
        if self.space is True:
            pygame.draw.rect(win, (255,0,0),  (self.x, self.y, self.width, self.height))
            
    def draw_left(self, win):
        pygame.draw.rect(win, (255, 255, 255), (self.x, self.y, self.width, self.height))
        if self.left is True:
            pygame.draw.rect(win, (255, 0, 0), (self.x, self.y, self.width, self.height))
            
    def draw_right(self, win):
        pygame.draw.rect(win, (255, 255, 255), (self.x, self.y, self.width, self.height))
        if self.right is True:
            pygame.draw.rect(win, (255, 0, 0), (self.x, self.y, self.width, self.height))

    def draw_shoot(self, win):
        pygame.draw.rect(win, (255,255,255), (self.x, self.y, self.width, self.height))
        if self.shoot is True:
            pygame.draw.rect(win, (255, 0, 0), (self.x, self.y, self.width, self.height))
#**********************************************************************************************
        
#text display
#**********************************************************************************************
class messages(object):
    def __init__(self, text, x, y):
        self.big_text = pygame.font.SysFont('free', 32) # free font haha
        self.small_text = pygame.font.SysFont('free', 14)
        self.death_text = pygame.font.SysFont('free', 64)
        self.x = x
        self.y = y
        self.score = self.big_text.render(text, True, (255,255,255))# the true is for anti aliasing
        self.space = self.small_text.render(text, True, (0,0,0))
        self.death = self.death_text.render(text, True, (0,0,0))
#***********************************************************************************************

#game window function
#****************************************GAME_WINDOW**************************************************
def game_window():
    global bgx
    global bgy
    global bgx_mid
    global bgy_mid
    
    #camera movement
    rel_x = bgx % screen_width
    win.blit(background, (rel_x - screen_width, bgy))
    if rel_x < screen_width:
        win.blit(background, (rel_x, 0))

    BDG.draw(win) # player

    # damage animation
    if not(BDG.can_take_damage()):
        if BDG.right is True:
            win.blit(player_sprite_hit_right, (BDG.x, BDG.y))
        elif BDG.left is True:
            win.blit(player_sprite_hit_left, (BDG.x, BDG.y))

    # draw objects
    chem.draw(win)
    chem_2.draw(win)
    chem_3.draw(win)
    chem_4.draw(win)
    chem_5.draw(win)
    chem_6.draw(win)
    chem_7.draw(win)
    chem_8.draw(win)

    bug_1.draw(win)
    bug_2.draw(win)
    bug_3.draw(win)
    bug_4.draw(win)
    bug_5.draw(win)

    boss_1.draw(win)

    heart_1.draw(win)
    heart_2.draw(win)
    heart_3.draw(win)
    heart_4.draw(win)
    
    plat1.draw(win)
    plat2.draw(win)
    plat3.draw(win)
    plat4.draw(win)
    plat5.draw(win)
    plat6.draw(win)
    plat7.draw(win)
    plat8.draw(win)
    plat9.draw(win)
    plat10.draw(win)
    plat11.draw(win)
    plat12.draw(win)
    plat13.draw(win)
    plat14.draw(win)
    plat15.draw(win)
    plat16.draw(win)
    plat17.draw(win)
    plat18.draw(win)
    plat19.draw(win)
    plat20.draw(win)
    plat21.draw(win)
    plat22.draw(win)
    plat23.draw(win)
    plat24.draw(win)
    plat25.draw(win)
    plat26.draw(win)
    
    for noodle in noodles:
        noodle.draw(win)

    for bullet in boss_shoot:
        bullet.draw(win)

    BDG.full_hearts(win)

    boss_1.health_bar(win)

    if boss_1.end_death is True:
        win_message = title_text.render("You Win!", True, (135,206,255))
        win.blit(win_message, (400, 200))
        
    space.draw_space(win)
    right_arrow.draw_right(win)
    left_arrow.draw_left(win)
    shoot.draw_shoot(win)

    score = messages("Score: " + str(BDG.score_counter), 25, 25)
    win.blit(score.score, (score.x, score.y))
    win.blit(space_message.space, (space_message.x, space_message.y))
    win.blit(R_message.space, (R_message.x, R_message.y))
    win.blit(L_message.space, (L_message.x, L_message.y))
    win.blit(shoot_message.space, (shoot_message.x, shoot_message.y))
    if boss_1.start is True:
        if boss_1.end_game is False:
            win.blit(boss_message.score, (boss_message.x, boss_message.y))

    #tile mapping
    y = 0
    for row in game_map:
        x = 0
        for col in row:
            if col == '1':
                win.blit(bridge_tile, (x  * tile_x + bgx, y * tile_y + bgy))
                bridge_hitbox = pygame.Rect(x  * tile_x + bgx, y * tile_y + bgy, 200, 25)
                #pygame.draw.rect(win, (255, 0, 0), bridge_hitbox, 2)
                if bridge_hitbox.colliderect(BDG.hitbox):
                    if BDG.y > 500 - BDG.height:
                        BDG.y = 500 - BDG.height
            if col == '3':
                hole_hitbox = pygame.Rect(x * tile_x + 18 + bgx, y* tile_y + bgy - 14, 200 - 36, 100 + 10)
                #pygame.draw.rect(win, (255,0,0), hole_hitbox, 2)
                if hole_hitbox.colliderect(BDG.hitbox):
                    BDG.isJump = False
                    if BDG.y < 600:
                        BDG.isJump = False
                        BDG.y += 10
                    if BDG.y >= 600:
                        BDG.death()
            if col == '4':
                if boss_1.end_game is True:
                    win.blit(cat_1, (x* tile_x + bgx, y * tile_y + bgy))
            if col == '5':
                if boss_1.end_game is True:
                    win.blit(cat_2, (x* tile_x + bgx, y * tile_y + bgy))
            x += 1
        y += 1
    # update
    pygame.display.update()
#********************************************************************************************
    
# classes and messages defined
# *******************************************************************************************
BDG = player(350, 100, 40, 40) #MUST BE OUTSIDE THE MAIN LOOP
chem = teacher(500, 461, 40, 40, 900)
chem_2 = teacher(2000, 461, 40, 40, 2400)
chem_3 = teacher(2700, 461, 40, 40, 3100)
chem_4 = teacher(5500, 397, 40, 40, 5725)
chem_5 = teacher(5900, 461, 40, 40, 6300)
chem_6 = teacher(8980, 461, 40, 40, 9780)
chem_7 = teacher(9380, 461, 40, 40, 9780)
chem_8 = teacher(9180, 461, 40, 40, 9580)
bug_1 = bug(700, 200, 36, 30, 1000)
bug_2 = bug(1650, 350, 36, 30, 1920)
bug_3 = bug(3800, 350, 36, 30, 4000)
bug_4 = bug(4200, 350, 36, 30, 4400)
bug_5 = bug(7375, 355, 36, 30, 8300)
boss_1 = boss(11700, 385, 84, 118)#11700
heart_1 = ground_hearts(2500, 458)
heart_2 = ground_hearts(5596, 394)
heart_3 = ground_hearts(7090, 140)
heart_4 = ground_hearts(9380, 458)
space = key_indicators(900, 15, 32, 12)
left_arrow = key_indicators(940, 15, 12, 12)
right_arrow = key_indicators(955, 15, 12, 12)
shoot = key_indicators(980, 15, 12, 12)
space_message = messages("SPACE", space.x, space.y)
R_message = messages("R", right_arrow.x, right_arrow.y)
L_message = messages("L", left_arrow.x, left_arrow.y)
shoot_message = messages("Z", shoot.x, shoot.y)
death_message = messages("YOU DIED", screen_width // 2, screen_height // 2)
boss_message = messages("ROBOMATO", 450, 95)

plat1 = plat(500, 375)#
plat2 = plat(625, 315)
plat3 = plat(750, 255)
plat4 = plat(875, 375)
plat5 = plat(1650, 450)#
plat6 = plat(1775, 400)
plat7 = plat(1900, 450)
plat8 = plat(3860, 400)#
plat9 = plat(4260, 400)
plat10 = plat(5500, 425)#
plat11 = plat(5575, 425)
plat12 = plat(5650, 425)
plat13 = plat(6450, 425)#
plat14 = plat(6575, 375)
plat15 = plat(6700, 325)
plat16 = plat(6825, 275)
plat17 = plat(6950, 225)
plat18 = plat(7075, 175)
plat19 = plat(8500, 425) #bottom right
plat20 = plat(8375, 355) #bottom left
plat21 = plat(8500, 285)
plat22 = plat(8375, 215)
plat23 = plat(8500, 145)
plat24 = plat(8650, 145)
plat25 = plat(8800, 145)
plat26 = plat(9870, 400)
#*********************************************************************************************
# ramen and hearts list
noodles = []
shoot_loop = 0
boss_shoot = []
'''
**********************************************MAIN LOOP*******************************************************
'''
main = True
while main:
    #pygame.time.delay(27)
    win.fill(0)
    game_window()
    clock.tick(72)

    if shoot_loop > 0:
        shoot_loop += 1
        
    if shoot_loop > 20:
        shoot_loop = 0

    for event in pygame.event.get():
        if event.type == pygame.QUIT: # if red x is hit
            main = False  
# shooting noodles
#****************************************************************************************************************************************************************
    for noodle in noodles: # run through each projectile the player has. The loop defines 'noodle'
        if noodle.hitbox.colliderect(chem.hitbox):
            if chem.visible is True:
                chem.hit()
                BDG.score_counter += 1
                chem.visible = False
                
        if noodle.hitbox.colliderect(chem_2.hitbox):
            if chem_2.visible is True:
                chem.hit()
                BDG.score_counter += 1
                chem_2.visible = False

        if noodle.hitbox.colliderect(chem_3.hitbox):
            if chem_3.visible is True:
                chem.hit()
                BDG.score_counter += 1
                chem_3.visible = False

        if noodle.hitbox.colliderect(chem_4.hitbox):
            if chem_4.visible is True:
                chem.hit()
                BDG.score_counter += 1
                chem_4.visible = False

        if noodle.hitbox.colliderect(chem_5.hitbox):
            if chem_5.visible is True:
                chem.hit()
                BDG.score_counter += 1
                chem_5.visible = False

        if noodle.hitbox.colliderect(chem_6.hitbox):
            if chem_6.visible is True:
                chem.hit()
                BDG.score_counter += 1
                chem_6.visible = False

        if noodle.hitbox.colliderect(chem_7.hitbox):
            if chem_7.visible is True:
                chem.hit()
                BDG.score_counter += 1
                chem_7.visible = False

        if noodle.hitbox.colliderect(chem_8.hitbox):
            if chem_8.visible is True:
                chem.hit()
                BDG.score_counter += 1
                chem_8.visible = False
        #****************BUGS************************

        if noodle.hitbox.colliderect(bug_1.hitbox):
            if bug_1.visible is True:
                bug_1.hit()
                BDG.score_counter += 1
                bug_1.visible = False


        if noodle.hitbox.colliderect(bug_2.hitbox):
            if bug_2.visible is True:
                bug_2.hit()
                BDG.score_counter += 1
                bug_2.visible = False

        if noodle.hitbox.colliderect(bug_3.hitbox):
            if bug_3.visible is True:
                bug_3.hit()
                BDG.score_counter += 1
                bug_3.visible = False

        if noodle.hitbox.colliderect(bug_4.hitbox):
            if bug_4.visible is True:
                bug_4.hit()
                BDG.score_counter += 1
                bug_4.visible = False

        if noodle.hitbox.colliderect(bug_5.hitbox):
            if bug_5.visible is True:
                bug_5.hit()
                BDG.score_counter += 1
                bug_5.visible = False
                
        if noodle.x < screen_width - 5 and noodle.x > 0 + 5:
            noodle.x += noodle.vel * facing
        else:
            try:
                noodles.pop(noodles.index(noodle))# remember from school. The pop thing searches for the specific noodle in the list and removes it
            except ValueError:
                shoot_loop = 1
#****************************************************************************************************************************************************************

#***********************************************************BOSS FIGHT*******************************************************************************************
        if boss_1.start is True :
            if noodle.hitbox.colliderect(boss_1.hitbox):
                if boss_1.visible is True:
                    boss_1.hit()
                    BDG.score_counter += 1
#******************************************************BOSS BULLETS********************************************************************************************
    for bullet in boss_shoot:
        if boss_1.start is True:
            if bullet.x > 9500:
                bullet.x -= bullet.vel
            else:
                boss_shoot.pop(boss_shoot.index(bullet))

#**************************************************Colliding with hearts******************************************************************************************
    if BDG.hitbox.colliderect(heart_1.hitbox):
        if heart_1.visible is True: 
            heart_1.visible = False
            BDG.get_health()

    if BDG.hitbox.colliderect(heart_2.hitbox):
        if heart_2.visible is True:
            heart_2.visible = False
            BDG.get_health()

    if BDG.hitbox.colliderect(heart_3.hitbox):
        if heart_3.visible is True:
            heart_3.visible = False
            BDG.get_health()

    if BDG.hitbox.colliderect(heart_4.hitbox):
        if heart_4.visible is True:
            heart_4.visible = False
            BDG.get_health()
#******************************************************************************************************************************************************************
    keys = pygame.key.get_pressed()

    if keys[pygame.K_RIGHT]:# the screen is divides into three sections each 333.3 pixels wide. if the player reaches the far right section the camera will move instead of the play
        if BDG.x >= screen_width - 333:
            bgx -= BDG.vel
            BDG.standing = False
        else:
            right_arrow.right = True # for key_display
            BDG.x += BDG.vel
            BDG.standing = False
            BDG.right = True # for animation
            BDG.left = False

    elif keys[pygame.K_LEFT]: # if the player reaches the far left section of the screen, the screen will move instead of the player. creating that illusion again
        if BDG.x <= screen_width - 666:
            bgx += BDG.vel
            BDG.standing = False
        else:
            BDG.x -= BDG.vel
            BDG.left = True
            BDG.right = False
            BDG.standing = False
            left_arrow.left = True
    else:
        BDG.standing = True
        BDG.walk_count = 0

    if len(noodles) < 3:
        if keys[pygame.K_z] and shoot_loop == 0:
            if BDG.left == True:
                facing = -1
            elif BDG.right == True:
                facing = 1
            shoot.shoot = True
            if len(noodles) < 3: # player can only have five bowls of ramen at a time
                noodles.append(ramen(round(BDG.x), round(BDG.y), 32, 32, facing))# makes sure the ramen always appears from the middle of the player
            shoot_loop = 1
            
    if boss_1.start is True:
        if boss_1.visible is True:
            shoot_timer = random.randint(0, 900) # a little slow
            shoot_y = random.randint(385, 485)
            if shoot_timer > 895:
                boss_shoot.append(boss_bullet(boss_1.x, shoot_y, 28, 36))
                print("SHOOT")
#*************************************************************************************************************************
    if not(BDG.isJump):
        if keys[pygame.K_SPACE]:
            if not(BDG.hitbox.colliderect(plat1.hitbox)):#Stops player from jumping in air. (if player is not on a plat)
                if not(BDG.hitbox.colliderect(plat2.hitbox)):
                    if not(BDG.hitbox.colliderect(plat3.hitbox)):
                        if not(BDG.hitbox.colliderect(plat4.hitbox)):
                            if not(BDG.hitbox.colliderect(plat5.hitbox)):
                                if not(BDG.hitbox.colliderect(plat6.hitbox)):
                                    if not(BDG.hitbox.colliderect(plat7.hitbox)):
                                        if not(BDG.hitbox.colliderect(plat8.hitbox)):
                                            if not(BDG.hitbox.colliderect(plat9.hitbox)):
                                                if not(BDG.hitbox.colliderect(plat10.hitbox)):
                                                    if not(BDG.hitbox.colliderect(plat11.hitbox)):
                                                        if not(BDG.hitbox.colliderect(plat12.hitbox)):
                                                            if not(BDG.hitbox.colliderect(plat13.hitbox)):
                                                                if not(BDG.hitbox.colliderect(plat14.hitbox)):
                                                                    if not(BDG.hitbox.colliderect(plat15.hitbox)):
                                                                        if not(BDG.hitbox.colliderect(plat16.hitbox)):
                                                                            if not(BDG.hitbox.colliderect(plat17.hitbox)):
                                                                                if not(BDG.hitbox.colliderect(plat18.hitbox)):
                                                                                    if not(BDG.hitbox.colliderect(plat19.hitbox)):
                                                                                        if not(BDG.hitbox.colliderect(plat20.hitbox)):
                                                                                            if not(BDG.hitbox.colliderect(plat21.hitbox)):
                                                                                                if not(BDG.hitbox.colliderect(plat22.hitbox)):
                                                                                                    if not(BDG.hitbox.colliderect(plat23.hitbox)):
                                                                                                        if not(BDG.hitbox.colliderect(plat24.hitbox)):
                                                                                                            if not(BDG.hitbox.colliderect(plat25.hitbox)):
                                                                                                                if not(BDG.hitbox.colliderect(plat26.hitbox)):
                                                                                                                    if BDG.y < 500 - BDG.height: # and if they're not on the ground
                                                                                                                        BDG.isJump = False #then they can't jump
                                                                                                                    else:
                                                                                                                        space.space = True
                                                                                                                        BDG.isJump = True
                                                                                                                        BDG.walkCount = 0
                                                                                                                else:
                                                                                                                    space.space = True
                                                                                                                    BDG.isJump = True
                                                                                                                    BDG.walkCount = 0
                                                                                                                    
                                                                                                            else:
                                                                                                                space.space = True
                                                                                                                BDG.isJump = True
                                                                                                                BDG.walkCount = 0
                                                                                                                
                                                                                                        else:
                                                                                                            space.space = True
                                                                                                            BDG.isJump = True
                                                                                                            BDG.walkCount = 0
                                                                                                            
                                                                                                    else:
                                                                                                        space.space = True
                                                                                                        BDG.isJump = True
                                                                                                        BDG.walkCount = 0
                                                                                                else:
                                                                                                    space.space = True
                                                                                                    BDG.isJump = True
                                                                                                    BDG.walkCount = 0
                                                                                            else:
                                                                                                space.space = True
                                                                                                BDG.isJump = True
                                                                                                BDG.walkCount = 0
                                                                                                
                                                                                        else:
                                                                                            space.space = True
                                                                                            BDG.isJump = True
                                                                                            BDG.walkCount = 0
                                                                                            
                                                                                    else:
                                                                                        space.space = True
                                                                                        BDG.isJump = True
                                                                                        BDG.walkCount = 0 
                                                                                else:
                                                                                    space.space = True
                                                                                    BDG.isJump = True
                                                                                    BDG.walkCount = 0
                                                                                    
                                                                            else:
                                                                                space.space = True
                                                                                BDG.isJump = True
                                                                                BDG.walkCount = 0
                                                                                
                                                                        else:
                                                                            space.space = True
                                                                            BDG.isJump = True
                                                                            BDG.walkCount = 0
                                                                            
                                                                    else:
                                                                        space.space = True
                                                                        BDG.isJump = True
                                                                        BDG.walkCount = 0
                                                                        
                                                                else:
                                                                    space.space = True
                                                                    BDG.isJump = True
                                                                    BDG.walkCount = 0
                                                                    
                                                            else:
                                                                space.space = True
                                                                BDG.isJump = True
                                                                BDG.walkCount = 0
                                                                
                                                        else:
                                                            space.space = True
                                                            BDG.isJump = True
                                                            BDG.walkCount = 0
                                                            
                                                    else:
                                                        space.space = True
                                                        BDG.isJump = True
                                                        BDG.walkCount = 0
                                                        
                                                else:
                                                    space.space = True
                                                    BDG.isJump = True
                                                    BDG.walkCount = 0
                                                    
                                            else:
                                                space.space = True
                                                BDG.isJump = True
                                                BDG.walkCount = 0
                                                
                                        else:
                                                space.space = True
                                                BDG.isJump = True
                                                BDG.walkCount = 0
                                            
                                    else:
                                        space.space = True
                                        BDG.isJump = True
                                        BDG.walkCount = 0
                                        
                                else:
                                        space.space = True
                                        BDG.isJump = True
                                        BDG.walkCount = 0
                                    
                            else:
                                space.space = True
                                BDG.isJump = True
                                BDG.walkCount = 0
                        else:
                                space.space = True
                                BDG.isJump = True
                                BDG.walkCount = 0
                            
                    else:
                            space.space = True
                            BDG.isJump = True
                            BDG.walkCount = 0
                        
                else:
                        space.space = True
                        BDG.isJump = True
                        BDG.walkCount = 0
                    
            else:
                    space.space = True
                    BDG.isJump = True
                    BDG.walkCount = 0
#*****************************************************************************************************************************
    #key_indicators
    if not(keys[pygame.K_RIGHT]):
        right_arrow.right = False

    if not(keys[pygame.K_LEFT]):
        left_arrow.left = False
        
    if not(keys[pygame.K_z]):
        shoot.shoot = False
        
    #Jumping
    if BDG.isJump == True:
        BDG.y -= BDG.jump_vel
        BDG.jump_vel -= 1
        if BDG.jump_vel < -16:
            BDG.isJump = False
            space.space = False
            BDG.jump_vel = 16
            
    #gravity
    if BDG.isJump is False:
        if not(BDG.hitbox.colliderect(plat1.hitbox)):
            if not(BDG.hitbox.colliderect(plat2.hitbox)):
                if not(BDG.hitbox.colliderect(plat3.hitbox)):
                    if not(BDG.hitbox.colliderect(plat4.hitbox)):
                        if not(BDG.hitbox.colliderect(plat5.hitbox)):
                            if not(BDG.hitbox.colliderect(plat6.hitbox)):
                                if not(BDG.hitbox.colliderect(plat7.hitbox)):
                                    if not(BDG.hitbox.colliderect(plat8.hitbox)):
                                        if not(BDG.hitbox.colliderect(plat9.hitbox)):
                                            if not(BDG.hitbox.colliderect(plat10.hitbox)):
                                                if not(BDG.hitbox.colliderect(plat11.hitbox)):
                                                    if not(BDG.hitbox.colliderect(plat12.hitbox)):
                                                        if not(BDG.hitbox.colliderect(plat13.hitbox)):
                                                            if not(BDG.hitbox.colliderect(plat14.hitbox)):
                                                                if not(BDG.hitbox.colliderect(plat15.hitbox)):
                                                                    if not(BDG.hitbox.colliderect(plat16.hitbox)):
                                                                        if not(BDG.hitbox.colliderect(plat17.hitbox)):
                                                                            if not(BDG.hitbox.colliderect(plat18.hitbox)):
                                                                                if not(BDG.hitbox.colliderect(plat19.hitbox)):
                                                                                    if not(BDG.hitbox.colliderect(plat20.hitbox)):
                                                                                        if not(BDG.hitbox.colliderect(plat21.hitbox)):
                                                                                            if not(BDG.hitbox.colliderect(plat22.hitbox)):
                                                                                                if not(BDG.hitbox.colliderect(plat23.hitbox)):
                                                                                                    if not(BDG.hitbox.colliderect(plat24.hitbox)):
                                                                                                        if not(BDG.hitbox.colliderect(plat25.hitbox)):
                                                                                                            if not(BDG.hitbox.colliderect(plat26.hitbox)):
                                                                                                                if BDG.y < 500 - BDG.height:
                                                                                                                    BDG.y += 16
                                                                                                                    if BDG.y > 600 - BDG.height:
                                                                                                                        BDG.y = 600 - BDG.height                
    #*************************PLAT  COLLISIONS*******************************************************************
    if plat1.hitbox.colliderect(BDG.hitbox):
        if BDG.jump_vel < 0:
            if BDG.isJump is True:
                BDG.isJump = False
                BDG.jump_vel = 16
                space.space = False
                BDG.hitbox.bottom = plat1.hitbox.top

    if plat2.hitbox.colliderect(BDG.hitbox):
        if BDG.jump_vel < 0:
            if BDG.isJump is True:
                BDG.isJump = False
                BDG.jump_vel = 16
                space.space = False
                BDG.hitbox.bottom = plat2.hitbox.top

    if plat3.hitbox.colliderect(BDG.hitbox):
        if BDG.jump_vel < 0:
            if BDG.isJump is True:
                BDG.isJump = False
                BDG.jump_vel = 16
                space.space = False
                BDG.hitbox.bottom = plat3.hitbox.top

    if plat4.hitbox.colliderect(BDG.hitbox):
        if BDG.jump_vel < 0:
            if BDG.isJump is True:
                BDG.isJump = False
                BDG.jump_vel = 16
                space.space = False
                BDG.hitbox.bottom = plat4.hitbox.top

    if plat5.hitbox.colliderect(BDG.hitbox):
        if BDG.jump_vel < 0:
            if BDG.isJump is True:
                BDG.isJump = False
                BDG.jump_vel = 16
                space.space = False
                BDG.hitbox.bottom = plat5.hitbox.top

    if plat6.hitbox.colliderect(BDG.hitbox):
        if BDG.jump_vel < 0:
            if BDG.isJump is True:
                BDG.isJump = False
                BDG.jump_vel = 16
                space.space = False
                BDG.hitbox.bottom = plat6.hitbox.top
                
    if plat7.hitbox.colliderect(BDG.hitbox):
        if BDG.jump_vel < 0:
            if BDG.isJump is True:
                BDG.isJump = False
                BDG.jump_vel = 16
                space.space = False
                BDG.hitbox.bottom = plat7.hitbox.top

    if plat8.hitbox.colliderect(BDG.hitbox):
        if BDG.jump_vel < 0:
            if BDG.isJump is True:
                BDG.isJump = False
                BDG.jump_vel = 16
                space.space = False
                BDG.hitbox.bottom = plat8.hitbox.top

    if plat9.hitbox.colliderect(BDG.hitbox):
        if BDG.jump_vel < 0:
            if BDG.isJump is True:
                BDG.isJump = False
                BDG.jump_vel = 16
                space.space = False
                BDG.hitbox.bottom = plat9.hitbox.top

    if plat10.hitbox.colliderect(BDG.hitbox):
        if BDG.jump_vel < 0:
            if BDG.isJump is True:
                BDG.isJump = False
                BDG.jump_vel = 16
                space.space = False
                BDG.hitbox.bottom = plat10.hitbox.top

    if plat11.hitbox.colliderect(BDG.hitbox):
        if BDG.jump_vel < 0:
            if BDG.isJump is True:
                BDG.isJump = False
                BDG.jump_vel = 16
                space.space = False
                BDG.hitbox.bottom = plat11.hitbox.top

    if plat12.hitbox.colliderect(BDG.hitbox):
        if BDG.jump_vel < 0:
            if BDG.isJump is True:
                BDG.isJump = False
                BDG.jump_vel = 16
                space.space = False
                BDG.hitbox.bottom = plat12.hitbox.top

    if plat13.hitbox.colliderect(BDG.hitbox):
        if BDG.jump_vel < 0:
            if BDG.isJump is True:
                BDG.isJump = False
                BDG.jump_vel = 16
                space.space = False
                BDG.hitbox.bottom = plat13.hitbox.top

    if plat14.hitbox.colliderect(BDG.hitbox):
        if BDG.jump_vel < 0:
            if BDG.isJump is True:
                BDG.isJump = False
                BDG.jump_vel = 16
                space.space = False
                BDG.hitbox.bottom = plat14.hitbox.top

    if plat15.hitbox.colliderect(BDG.hitbox):
        if BDG.jump_vel < 0:
            if BDG.isJump is True:
                BDG.isJump = False
                BDG.jump_vel = 16
                space.space = False
                BDG.hitbox.bottom = plat15.hitbox.top
                
    if plat16.hitbox.colliderect(BDG.hitbox):
        if BDG.jump_vel < 0:
            if BDG.isJump is True:
                BDG.isJump = False
                BDG.jump_vel = 16
                space.space = False
                BDG.hitbox.bottom = plat16.hitbox.top

    if plat17.hitbox.colliderect(BDG.hitbox):
        if BDG.jump_vel < 0:
            if BDG.isJump is True:
                BDG.isJump = False
                BDG.jump_vel = 16
                space.space = False
                BDG.hitbox.bottom = plat17.hitbox.top

    if plat18.hitbox.colliderect(BDG.hitbox):
        if BDG.jump_vel < 0:
            if BDG.isJump is True:
                BDG.isJump = False
                BDG.jump_vel = 16
                space.space = False
                BDG.hitbox.bottom = plat18.hitbox.top

    if plat19.hitbox.colliderect(BDG.hitbox):
        if BDG.jump_vel < 0:
            if BDG.isJump is True:
                BDG.isJump = False
                BDG.jump_vel = 16
                space.space = False
                BDG.hitbox.bottom = plat19.hitbox.top

    if plat20.hitbox.colliderect(BDG.hitbox):
        if BDG.jump_vel < 0:
            if BDG.isJump is True:
                BDG.isJump = False
                BDG.jump_vel = 16
                space.space = False
                BDG.hitbox.bottom = plat20.hitbox.top

    if plat21.hitbox.colliderect(BDG.hitbox):
        if BDG.jump_vel < 0:
            if BDG.isJump is True:
                BDG.isJump = False
                BDG.jump_vel = 16
                space.space = False
                BDG.hitbox.bottom = plat21.hitbox.top

    if plat22.hitbox.colliderect(BDG.hitbox):
        if BDG.jump_vel < 0:
            if BDG.isJump is True:
                BDG.isJump = False
                BDG.jump_vel = 16
                space.space = False
                BDG.hitbox.bottom = plat22.hitbox.top

    if plat23.hitbox.colliderect(BDG.hitbox):
        if BDG.jump_vel < 0:
            if BDG.isJump is True:
                BDG.isJump = False
                BDG.jump_vel = 16
                space.space = False
                BDG.hitbox.bottom = plat23.hitbox.top

    if plat24.hitbox.colliderect(BDG.hitbox):
        if BDG.jump_vel < 0:
            if BDG.isJump is True:
                BDG.isJump = False
                BDG.jump_vel = 16
                space.space = False
                BDG.hitbox.bottom = plat24.hitbox.top

    if plat25.hitbox.colliderect(BDG.hitbox):
        if BDG.jump_vel < 0:
            if BDG.isJump is True:
                BDG.isJump = False
                BDG.jump_vel = 16
                space.space = False
                BDG.hitbox.bottom = plat25.hitbox.top

    if plat26.hitbox.colliderect(BDG.hitbox):
        if BDG.jump_vel < 0:
            if BDG.isJump is True:
                BDG.isJump = False
                BDG.jump_vel = 16
                space.space = False
                BDG.hitbox.bottom = plat26.hitbox.top
    #******************************************************************************

    #**********************TEACHER & BUG COLLISIONS********************************
    if BDG.hitbox.colliderect(chem.hitbox):
        if chem.visible is True:
            BDG.hit()

    if BDG.hitbox.colliderect(chem_2.hitbox):
        if chem_2.visible is True:
            BDG.hit()

    if BDG.hitbox.colliderect(chem_3.hitbox):
        if chem_3.visible is True:
            BDG.hit()

    if BDG.hitbox.colliderect(chem_4.hitbox):
        if chem_4.visible is True:
            BDG.hit()

    if BDG.hitbox.colliderect(chem_5.hitbox):
        if chem_5.visible is True:
            BDG.hit()

    if BDG.hitbox.colliderect(chem_6.hitbox):
        if chem_6.visible is True:
            BDG.hit()

    if BDG.hitbox.colliderect(chem_7.hitbox):
        if chem_7.visible is True:
            BDG.hit()

    if BDG.hitbox.colliderect(chem_8.hitbox):
        if chem_8.visible is True:
            BDG.hit()
            
    #********************BUG*******************************************************
    if BDG.hitbox.colliderect(bug_1.hitbox):
        if bug_1.visible is True:
            BDG.hit()

    if BDG.hitbox.colliderect(bug_2.hitbox):
        if bug_2.visible is True:
            BDG.hit()
            
    if BDG.hitbox.colliderect(bug_3.hitbox):
        if bug_3.visible is True:
            BDG.hit()

    if BDG.hitbox.colliderect(bug_4.hitbox):
        if bug_4.visible is True:
            BDG.hit()

    if BDG.hitbox.colliderect(bug_5.hitbox):
        if bug_5.visible is True:
            BDG.hit()
    #************************************************************************

    #*******************BOSS*************************************************
    if boss_1.start is True:
        if BDG.hitbox.colliderect(boss_1.hitbox):
            if boss_1.visible is True:
                BDG.hit()

    if boss_1.x <= 10000:
        BDG.death()
        boss_1.start = False

    for bullet in boss_shoot: # run through each projectile the player has. The loop defines 'noodle'
        if bullet.hitbox.colliderect(BDG.hitbox):
            if boss_1.visible is True:
                BDG.hit()                
    #************************************************************************
    #borders
    right_border = pygame.Rect(11800 + bgx, 0 + bgy, 200, 600) # - 1800
    if right_border.colliderect(BDG.hitbox):
        BDG.x -= BDG.vel # right

    if BDG.y > 600 - BDG.height:
        BDG.death() # bottom
        
    left_border = pygame.Rect(-200 + bgx, 0 + bgy, 200, 600) # left 
    if left_border.colliderect(BDG.hitbox):
        BDG.x += BDG.vel

    if BDG.y < 0:
        BDG.y += BDG.vel # top

    if boss_1.start is True:
        boss_border = pygame.Rect(9800 + bgx, 0 + bgy, 200, 600)
        if boss_border.colliderect(BDG.hitbox):
            BDG.x += BDG.vel
        
    #print("BGX: " + str(bgx))
    #print("BGX_mid " + str(bgx_mid))
'''
******************************************************************************************************************************************************
'''
pygame.quit()
