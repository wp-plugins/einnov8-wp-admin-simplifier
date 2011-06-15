<?php
/*
Plugin Name: eInnov8 WP Admin Simplifier
Plugin URI: http://wordpress.org/extend/plugins/einnov8-wp-admin-simplifier/
Plugin Description: Customize appearance of admin panels and limit options for non-admin users.
Version: 2.0.6
Author: Tim Gallaugher
Author URI: http://wordpress.org/extend/plugins/profile/yipeecaiey
License: GPL2 

Copyright 2010 eInnov8 Marketing  (email : timg@einnov8.com)

This program is free software; you can redistribute it and/or modify
it under the terms of the GNU General Public License, version 2, as 
published by the Free Software Foundation.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program; if not, write to the Free Software
Foundation, Inc., 51 Franklin St, Fifth Floor, Boston, MA  02110-1301  USA
*/


//add the new admin css
function ei8_admin_css() {
    $url = get_settings('siteurl');
    $siteType = ei8_admin_get_site_type();
    if ($siteType) echo '<link rel="stylesheet" href="'.$url.'/wp-content/plugins/'.ei8_admin_get_plugin_dir().'/css/'.$siteType.'_wp-admin.css" type="text/css" />';
}
add_action('admin_head','ei8_admin_css');

//add the login css
function ei8_admin_login_css() {
    $url = get_settings('siteurl');
    $siteType = ei8_admin_get_site_type();
    if ($siteType) echo '<link rel="stylesheet" href="'.$url.'/wp-content/plugins/'.ei8_admin_get_plugin_dir().'/css/'.$siteType.'_login.css" type="text/css" />';
}
add_action('login_head','ei8_admin_login_css');

//alter the login logo
function ei8_admin_login_form() {
    $siteType = ei8_admin_get_site_type();
    if (!$siteType) return false;
    switch($siteType) {
        case "flood":
            $title = "Web Content Flood - Comprehensive done-for-you search engine marketing system";
            $url   = "http://webcontentflood.com/";
            break;
        case "videoaudio":
            $title = "Video-Audio Forums - Interaction Technology For Education and Business";
            $url   = "http://videoaudioforums.com";
            break;
        case "soupedup":
            $title = "Souped Up Blogs - Blog Technology For The Serious Internet Marketer";
            $url   = "http://soupedupblogs.com/";
            break;
        case "boon":
        default:
            $title = "testiBOONials - Simple collection and posting of testimonials";
            $url   = "http://testiBOONials.com";
    }
    $wpurl = get_bloginfo('wpurl');
    echo '<script type="text/javascript">
    els = document.getElementsByTagName("h1"); 
    els[0].innerHTML = \'<h1><a title="'.$title.'" href="'.$url.'">'.$title.'</a></h1>\';
</script>';
}
add_action('login_form','ei8_admin_login_form');

//add the javascript to remove the footer
$js = (ei8_admin_get_option('ei8_admin_show_footer')=="show") ? "remove_footer_partial.js" : "remove_footer.js" ;
wp_enqueue_script( 'ei8_admin_remove_footer', WP_PLUGIN_URL.'/'.ei8_admin_get_plugin_dir().'/js/'.$js, array('jquery') );

//handle redirects
function ei8_admin_redirect($goto) {
    if ( !headers_sent() ) {
    	wp_redirect($goto);
    	exit();
    } else {
    	$url = admin_url($goto);
    
    	?>
    
    	<meta http-equiv="Refresh" content="0; URL=<?php echo $url; ?>">
    	<script type="text/javascript">
    	<!--
    		document.location.href = "<?php echo $url; ?>"
    	//-->
    	</script>
    	</head>
    	<body>
    	Sorry. Please use this <a href="<?php echo $url; ?>">link</a>.
    	</body>
    	</html>
    
    	<?php
    	exit();
    }
}

// Limited interface
function ei8_admin_limit_interface() {
    global $parent_file;
    $siteType = ei8_admin_get_site_type();
    $soupedUp = ($siteType=="soupedup") ? true : false ;
    
    $disallowed_pages = array('edit.php?post_status=draft');
    
	if (!current_user_can('level_10')) {
	    //handle redirection
	    if (!$soupedUp && $parent_file == 'index.php') ei8_admin_redirect('edit.php?post_status=draft');
		
		//hide specific menu items using css
    	echo "<style type='text/css'>";    
    	if (!$soupedUp) {
    	    echo "li#menu-dashboard { display:none; }";
        	echo "li#menu-appearance { display: none; }";    
        	//echo "li#menu-pages { display:none; }";    
        	echo "li#menu-links { display:none; }";
        	echo "li#menu-media { display:none; }";
        	//echo "li#menu-users { display:none; }";
        	echo "li#menu-comments { display:none; }";
        }
    	echo "li#menu-tools { display:none; }";    	

        //hide items in header
    	//echo "h1#site-heading { display:none; }";
    	echo "#site-title { display:none; }";
    	echo "#user_info { display:none; }";
    	echo "#favorite-actions { display:none; }";
    	//echo "#screen-options-link-wrap { display:none; }";
    	echo "#screen-meta-links { display:none; }";

    	if (!$soupedUp) { 
        	//hide elements on edit page screens
        	echo "div#pagecustomdiv { display:none; }";
        	echo "div#pagecommentstatusdiv { display:none; }";
        	echo "div#pageauthordiv { display:none; }";
        	echo "div#pageparentdiv { display:none; }";
        	echo "div#cftdiv { display:none; }";
        	
        	//hide elements on edit post screens
        	echo "div#postexcerpt { display:none; }";
        	echo "div#trackbacksdiv { display:none; }";
        	echo "div#commentsdiv { display:none; }";
        	echo "div#postcustom { display:none; }";
        	echo "div#commentstatusdiv { display:none; }";
        	echo "div#authordiv { display:none; }";
    	}
?>
#small_user_info {
	float: right;
	font-size: 12px;
	line-height: 46px;
	height: 46px;
	position:absolute;
	right:15px;
	top:0;
}

#small_user_info a, #small_user_info a:visited {
	color: #ccc;
	text-decoration: none;
}

#small_user_info p {
	margin: 0;
	padding: 0;
	line-height: 46px;
}

#favorite-actions {
	margin-right: 100px;
}    	
</style>
<?php
    	
    	//remove specific items from menu arrays before they are displayed
    	global $menu, $submenu;
    	$skipItems = ($soupedUp) ? array() : array(
    	    'edit-tags.php?taxonomy=post_tag',
    	    'edit-tags.php',
    	    'categories.php',
    	    'profile.php',
    	);
        foreach ($menu as $index => $item) {
			if ($item == 'index.php')
				continue;

			if ( isset($submenu) && !empty($submenu[$item[2]]) ) {
    			foreach ($submenu[$item[2]] as $subindex => $subitem) {
    			    //echo "subitem $subindex: </pre>";
    			    //print_r($subitem);
    			    //echo "</pre>";
    			    if(in_array($subitem[2], $skipItems))
    					unset($submenu[$item[2]][$subindex]);
    			}
    		}
        }
        
        //alter the user-info area in top right
        echo '<script type="text/javascript">
        jQuery(document).ready(function() { 
            jQuery(\'#favorite-actions\').remove();
            jQuery(\'#user_info\').remove();
            jQuery(\'div#wpcontent\').after(\'<div id="small_user_info"><p><a href="' . ei8_admin_get_blog_option('siteurl') . wp_nonce_url( ('/wp-login.php?action=logout'), 'log-out' ) . '" title="' . __('Log Out') . '">' . __('Log Out') . '</a></p></div>\');
        });
   </script>';
	}
}
add_action('admin_head','ei8_admin_limit_interface');

function ei8_admin_change_pass_init()
{
	add_submenu_page( 'users.php', __('Change Password', 'change-password-email'), __('Change Password', 'change-password-email'), 0, 'change-password', 'ei8_admin_change_password' );
}

function ei8_admin_change_pass_check()
{
	if( isset($_GET['page']) && $_GET['page']=='change-password' )
	{
		wp_enqueue_script('user-profile');
		wp_enqueue_script('password-strength-meter');
	}
}

function ei8_admin_change_password()
{
	$title = __('Change Password');
	$what = 'change-password';
	
	wp_reset_vars(array('action', 'redirect', 'profile', 'user_id', 'wp_http_referer'));
	
	$wp_http_referer = remove_query_arg(array('update', 'delete_count'), stripslashes($wp_http_referer));
	
	$user_id = (int) $user_id;
	
	if ( !$user_id ) {
		$current_user = wp_get_current_user();
		$user_id = $current_user->ID;
	} elseif ( !get_userdata($user_id) ) {
		wp_die( __('Invalid user ID.') );
	}
	
	if(!empty($_POST['action']) && $_POST['action']=='update') {
	
    	check_admin_referer('update-user_' . $user_id);
    	
    	if ( !current_user_can('edit_user', $user_id) )
    		wp_die(__('You do not have permission to edit this user.'));
    	
    	do_action('personal_options_update', $user_id);
    	global $wpdb;
    	if( !empty($_POST['pass1']) ) {
    	    if(!empty($_POST['pass2']) && $_POST['pass1']==$_POST['pass2'])
    		    $errors = $wpdb->query("UPDATE $wpdb->users SET user_pass = '".wp_hash_password($_POST['pass1'])."' WHERE ID = $user_id");
    		else $errorMsg = "Passwords do not match.";
        }
    	
    	if ( $errors ) {
    	    $redirect = "profile.php?page=".$what."&updated=true";
    		//wp_redirect($redirect);
            ei8_admin_redirect($redirect); 
    		//echo '<div id="message" class="updated fade"><p><strong>Password successfully updated.</strong></p></div>';
    		exit;
    	}
    }
	
	//default form
	$profileuser = get_user_to_edit($user_id);
	
	if ( !current_user_can('edit_user', $user_id) )
		wp_die(__('You do not have permission to edit this user.'));
	?>
	
	<?php if ( isset($_GET['updated']) ) : ?>
	<div id="message" class="updated fade">
		<p><strong><?php _e('User updated.') ?></strong></p>
	</div>
	<?php endif; ?>
	
	<?php if ( isset( $errors ) && is_wp_error( $errors ) ) : ?>
	<div class="error">
		<ul>
		<?php
		foreach( $errors->get_error_messages() as $message )
			echo "<li>$message</li>";
		?>
		</ul>
	</div>
	<?php endif; ?>
	
	<?php if ( isset( $errorMsg ) ) echo '<div class="error"><p>'.$errorMsg.'</p></div>'; ?>
	
	<div class="wrap" id="profile-page">
	<?php screen_icon(); ?>
	<h2><?php echo $title; ?></h2>
	
	<form id="your-profile" action="<?php echo "profile.php?page=".$what; ?>" method="post">
	<?php wp_nonce_field('update-user_' . $user_id) ?>
	<?php if ( $wp_http_referer ) : ?>
		<input type="hidden" name="wp_http_referer" value="<?php echo esc_url($wp_http_referer); ?>" />
	<?php endif; ?>
	<p>
	<input type="hidden" name="from" value="profile" />
	<input type="hidden" name="checkuser_id" value="<?php echo $user_ID ?>" />
	</p>
	
	<?php
	$show_password_fields = apply_filters('show_password_fields', true, $profileuser);
	$siteName = ei8_admin_get_site_type_name();
	if ( $show_password_fields ) :
	?>
	<p>To change your password to access the admin
area of your <?php echo $siteName; ?> account, please enter your new
password twice below</p>

    <table class="form-table">	
	<tr id="password">
		<th><label for="pass1">Enter New Password: </label></th>
		<td><input type="password" name="pass1" id="pass1" size="16" value="" autocomplete="off" /></td>
	</tr>	
	<tr id="password">
		<th><label for="pass2">Confirm New Password: </label></th>
		<td><input type="password" name="pass2" id="pass2" size="16" value="" autocomplete="off" /></td>
	</tr>
	</table>
	<?php endif; ?>
	<p class="submit">
		<input type="hidden" name="action" value="update" />
		<input type="hidden" name="user_id" id="user_id" value="<?php echo $user_id; ?>" />
		<input type="submit" class="button-primary" value="<?php _e('Update Profile') ?>" name="submit" />
	</p>
	</form>
	</div>
<?php
}
add_action('admin_menu','ei8_admin_change_pass_init');
add_action('admin_init','ei8_admin_change_pass_check');

//create options page
add_action('admin_menu', 'ei8_admin_options_menu');

function ei8_admin_options_menu() {
    $siteName = ei8_admin_get_site_type_name();
    add_options_page($siteName.' Admin Simplifier Preferences', 'Admin Simplifier', 10, __FILE__, 'ei8_admin_admin_options');
}

function ei8_admin_admin_options() {
    $url = "options-general.php?page=" . plugin_basename( __FILE__ );
    if($_POST['action']=="update") {
        $var = 'ei8_admin_site_type';
        ei8_admin_update_option($var, $_POST[$var]);
        
        $var = 'ei8_admin_show_footer';
        ei8_admin_update_option($var, $_POST[$var]);
        
        $siteName = ei8_admin_get_site_type_name();
        if($siteName=="None") $siteName = "website";
        ei8_admin_admin_log("<p>Your $siteName preferences have been updated.</p>",1);
        
        //force page reload
        if ( !headers_sent() ) {
			wp_redirect($url);
		} else {
			$url = admin_url($url);

?>

			<meta http-equiv="Refresh" content="0; URL=<?php echo $url; ?>">
			<script type="text/javascript">
			<!--
				document.location.href = "<?php echo $url; ?>"
			//-->
			</script>
			</head>
			<body>
			Sorry. Please use this <a href="<?php echo $url; ?>" title="New Post">link</a>.
			</body>
			</html>

<?php
		}
		exit();
        
        
    }

$siteType = ei8_admin_get_site_type();
$showFooter = ei8_admin_get_option('ei8_admin_show_footer');
?>
<div class="wrap">
	<?php screen_icon(); ?>
	
    <h2>Admin Simplifier Preferences:</h2>
    <form method="post" action="<?php echo $url; ?>">
    <table class="form-table">
        <tr valign="top">
            <th scope="row">Website type: </th>
            <td><select name='ei8_admin_site_type'>
                    <option value="" <?php if (!$siteType) echo "SELECTED"; ?>><?php echo ei8_admin_get_site_type_name('none'); ?></option>
                    <option value="boon" <?php if('boon'==$siteType) echo "SELECTED"; ?>><?php echo ei8_admin_get_site_type_name('boon'); ?></option>
<!--                     <option value="flood" <?php if('flood'==$siteType) echo "SELECTED"; ?>><?php echo ei8_admin_get_site_type_name('flood'); ?></option>
 -->                    <option value="videoaudio" <?php if('videoaudio'==$siteType) echo "SELECTED"; ?>><?php echo ei8_admin_get_site_type_name('videoaudio'); ?></option>
                    <option value="soupedup" <?php if('soupedup'==$siteType) echo "SELECTED"; ?>><?php echo ei8_admin_get_site_type_name('soupedup'); ?></option>
                </select></td>
        </tr>
        <tr valign="top">
            <th scope="row">Hide Footer: </th>
            <td><select name='ei8_admin_show_footer'>
                    <option value="hide" <?php if (!$showFooter || $showFooter=="hide") echo "SELECTED"; ?>>Hide footer (default)</option>
                    <option value="show" <?php if('show'==$showFooter) echo "SELECTED"; ?>>Show footer</option>
                </select></td>
        </tr>
    </table>    
    <input type="hidden" name="action" value="update">
    <p class="submit"><input type="submit" class="button-primary" value="<?php _e('Save Changes') ?>" /></p>
    </form>
</div>
<?php
}

function ei8_admin_get_plugin_dir() {
    $pathinfo = pathinfo( plugin_basename( __FILE__ ) );
    $pluginDir = $pathinfo['dirname'];
    return $pluginDir;
}

function ei8_admin_get_blog_option($val) {
    global $wp_version;
    return ($wp_version >= 3) ? get_site_option($val) : get_blog_option($val) ;
} 

function ei8_admin_get_option($id) {
    global $wpdb;
    $wpdb->flush();
    $table   = $wpdb->prefix . "ei8_admin_options";
    $sql     = "SELECT option_value FROM $table WHERE option_name='$id' LIMIT 1";
    $results = $wpdb->get_results($sql);
    $result = stripslashes($results[0]->option_value);
    return $result;
}
    
function ei8_admin_update_option($id, $value) {
    global $wpdb;
    $table = $wpdb->prefix . "ei8_admin_options";
    //check first to see if the option already exists
    $sql = "SELECT ID FROM $table WHERE option_name='$id'";
    $results = $wpdb->get_results($sql);
    $option_id = $results[0]->ID;
    $value = addslashes($value);
    if(!empty($option_id)) {
        $sql = "UPDATE $table SET option_value='$value' WHERE ID='$option_id'";
    } else {
        $sql = "INSERT INTO $table SET option_name='$id', option_value='$value'
    ON DUPLICATE KEY UPDATE option_value='$value'";
    }
    $wpdb->query($sql);
    $wpdb->flush();
}

function ei8_admin_get_site_type() {
    $siteType = ei8_admin_get_option('ei8_admin_site_type');
    if(empty($siteType)) {
        $domain = $_SERVER['HTTP_HOST'];
        if(ereg('1tme1',$domain))     $siteType = 'boon';
        elseif(ereg('1wcf1',$domain)) $siteType = 'flood';
        elseif(ereg('1vaf',$domain))  $siteType = 'videoaudio';
        elseif(ereg('1blg1',$domain)) $siteType = 'soupedup';
        elseif(ereg('local',$domain)) $siteType = 'flood';
        else                          $siteType = false; //'boon';
    }
    return $siteType;
}

function ei8_admin_get_site_type_name($siteType='') {
    if(empty($siteType)) $siteType = ei8_admin_get_site_type();
    //$siteName = ($siteType=="flood") ? 'webcontentFLOOD' : 'testiBOONials' ;
    switch ($siteType) {
        case "flood":
            $siteName = "webcontentFLOOD";
            break;
        case "videoaudio":
            $siteName = "Video-Audio Forums";
            break;
        case "soupedup":
            $siteName = "Souped-Up Blogs";
            break;
        case "boon":
            $siteName = "testiBOONials";
            break;
        default:
            $siteName = "None";
    }
            
    return $siteName;
}

//create admin link to settings from main app plugins page
add_filter( 'plugin_action_links', 'ei8_admin_settings_link', 10, 2 );

function ei8_admin_settings_link($links, $file) {  
    $file_name   = plugin_basename( __FILE__ );
	if ( $file == $file_name ) {
		array_unshift( $links, sprintf( '<a href="options-general.php?page=%s">%s</a>', $file_name, __('Settings') ) );
	}	
	return $links;
}

//handle db table installs and updates
function ei8_admin_admin_install() {
    global $wpdb, $wp_version;
    
    $table1 = $wpdb->prefix . "ei8_admin_options";

    $table1_sql = "CREATE TABLE `{$table1}` (
        `ID` BIGINT( 20 ) NOT NULL AUTO_INCREMENT,
        `option_name` VARCHAR( 100 ) NOT NULL ,
        `option_value` VARCHAR( 255 ) NOT NULL,
        PRIMARY KEY ( `ID` ),
        UNIQUE ( `option_name` )
        );";

    if($wp_version < 3) require_once(ABSPATH . 'wp-admin/includes/upgrade.php');
    $ei8_admin_db_sql   = $table1_sql ;
    
    $wpdb->flush();
    $errs = 0;
    
    //first check for old testiboonials settings
    $table2 = $wpdb->prefix . "testiboonials_admin_options";
    if($wpdb->get_var("SHOW TABLES LIKE '$table2'")==$table2 && $wpdb->get_var("SHOW TABLES LIKE '$table1'") != $table1) {
        ei8_admin_admin_log("<p>Converting database from older version.</p>",1);

        if ($wpdb->get_var("SHOW TABLES LIKE '$table1'") != $table1) {
            $sql = "CREATE TABLE $table1 LIKE $table2";
            $errs += ei8_admin_admin_query($sql);
            
            if($errs<1) {
                $sql = "SELECT * FROM $table2";
                $myrows = $wpdb->get_results($sql);
                foreach($myrows AS $row) {
                    
                //ei8_admin_admin_log("<p class='abq-error'>row: $row</p>",1);
                    //list($id,$name,$val) = $row;
                    $id   = $row->ID;
                    $name = $row->option_name;
                    $val  = $row->option_value;
                    $name = str_replace('testiboonials','ei8',$name);
                    $val  = str_replace('testiboonials','ei8',$val);
                    $sql  = "INSERT INTO $table1 SET ID=$id, option_name='$name', option_value='$val'";
                    $errs += $wpdb->query($sql);
                }
                $sql = "DROP table $table2";
                $errs += ei8_admin_admin_query($sql);
                
                ei8_admin_admin_log("<p>Storing new table structure</p>");
                update_site_option("ei8_admin_db_sql",$ei8_admin_db_sql);
                
                ei8_admin_admin_log("<p>Database tables converted</p>",1);
            }
        } else {
            ei8_admin_admin_log("<p class='abq-error'>Errors converting database</p>",1);
        }                
    
    //handle first time installs
    } elseif($wpdb->get_var("SHOW TABLES LIKE '$table1'") != $table1) {
        ei8_admin_admin_log("<p>Performing initial database installation.</p>",1);
            
        ei8_admin_admin_log("<p>Creating new tables</p>");
        $errs += ei8_admin_admin_query($table1_sql);
        
        if($errs<1) {
            ei8_admin_admin_log("<p>Storing new table structure</p>");
            update_site_option("ei8_admin_db_sql",$ei8_admin_db_sql);
            
            ei8_admin_admin_log("<p>New database tables installed</p>",1);
        } else {
            ei8_admin_admin_log("<p class='abq-error'>Errors installing new database tables</p>",1);
        }

    //handle upgrades
    } elseif( ei8_admin_get_blog_option( "ei8_admin_db_sql" ) != $ei8_admin_db_sql ) {
        ei8_admin_admin_log("<p>Previous database version found...performing database upgrade</p>",1);
        
        //create table backups
        ei8_admin_admin_log("<p>Backing up current tables</p>");        
        $table1_bak = $table1."_bak";
        $errs += ei8_admin_admin_query( "RENAME TABLE $table1 TO $table1_bak;" );
        
        //create new tables
        ei8_admin_admin_log("<p>Creating new tables</p>");
        $errs += ei8_admin_admin_query($table1_sql);
        
        //copy data from backups
        ei8_admin_admin_log("<p>Copying old data into new tables</p>");        
        $errs += ei8_admin_admin_query( "INSERT INTO $table1 SELECT * FROM $table1_bak;" );
        
        //drop backup tables
        ei8_admin_admin_log("<p>Dropping backup tables</p>");        
        $errs += ei8_admin_admin_query( "DROP TABLE $table1_bak;" );
        
        //update options db_version
        if($errs<1) {
            ei8_admin_admin_log("<p>Storing new table structure</p>");
            update_site_option("ei8_admin_db_sql",$ei8_admin_db_sql);
            ei8_admin_admin_log("<p>Database tables updated to current version</p>",1);
        } else {
            ei8_admin_admin_log("<p class='abq-error'>Errors updating database</p>",1);
        }
        
    } else {
        //ei8_admin_admin_log("<p>Database is up to date. No updates performed.</p>",1);
    }
}


//logging functions
//install messages are stored as an option in the db as a page refresh occurs after the plugin is installed and before the page is renedered...effectively disabling any install reporting

//add msg to stored list
function ei8_admin_admin_log( $msg, $level=2 ) {
    global $ei8_admin_debug;
    if( $level<2 || isset($ei8_admin_debug) ) {
        //$logMsg .= $msg;
        update_site_option('ei8_admin_admin_log', ei8_admin_get_blog_option('ei8_admin_admin_log') . $msg);
    }
}

//execute sql queries, check for and log any sql errors
//returns 1 if errors are found, 0 if none
function ei8_admin_admin_query($sql) {
    global $wpdb, $ei8_admin_debug;
    
    //conditionally turn on error reporting
    //NOTE: there has got to be a better way to catch and display sql errors...but I ran out of time...
    if (isset($ei8_admin_debug)) $wpdb->show_errors(); 
       
    return ( $wpdb->query($sql) === FALSE ) ? 1 : 0 ;
}

//retrieve and display logMsg if it exists
function ei8_admin_admin_notices() {
    //$title = "<p><strong>Testiboonials XMLRPC notifier has been updated.</strong></p>";
    $msg   = ei8_admin_get_blog_option('ei8_admin_admin_log');
    if(!empty($msg)) {
        echo "<div id='akismet-warning' class='updated fade'>" . $title . $msg . "</div>";
        update_site_option( 'ei8_admin_admin_log', '' );
    }
}

//uncomment this line below to enable verbose install logging & display sql errors
$ei8_admin_debug = 1;
    
add_action('admin_init', 'ei8_admin_admin_install');
add_action('admin_notices', 'ei8_admin_admin_notices');

?>