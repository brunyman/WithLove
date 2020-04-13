package net.craftersland.restarter.events;

import java.util.Optional;

import org.spongepowered.api.Sponge;
import org.spongepowered.api.data.key.Keys;
import org.spongepowered.api.entity.living.player.Player;
import org.spongepowered.api.entity.living.player.gamemode.GameModes;
import org.spongepowered.api.event.Listener;
import org.spongepowered.api.event.command.SendCommandEvent;
import org.spongepowered.api.event.network.ClientConnectionEvent;
import org.spongepowered.api.item.ItemTypes;
import org.spongepowered.api.item.inventory.ItemStack;
import org.spongepowered.api.service.ProviderRegistration;
import org.spongepowered.api.text.Text;

import net.craftersland.restarter.RR;
import net.luckperms.api.LuckPerms;
import net.luckperms.api.cacheddata.CachedPermissionData;
import net.luckperms.api.context.ContextManager;
import net.luckperms.api.context.ImmutableContextSet;
import net.luckperms.api.model.data.DataMutateResult;
import net.luckperms.api.model.user.User;
import net.luckperms.api.node.Node;
import net.luckperms.api.query.QueryOptions;

public class HacksFixes {
	
	private RR pl;
	private LuckPerms permsAPI;
	
	
	public HacksFixes(RR pl) {
		this.pl = pl;
		Optional<ProviderRegistration<LuckPerms>> provider = Sponge.getServiceManager().getRegistration(LuckPerms.class);
		if (provider.isPresent()) {
		    this.permsAPI = provider.get().getProvider();
		    
		}
	}
	
	@Listener
	public void onCMD(SendCommandEvent event) {
		if (pl.getConfigHandler().getBoolean("Anti-Hacks-Protection") == false) return;
		final Optional<Player> p = event.getCause().first(Player.class);
		if (p.isPresent()) {
			for (String cmd : pl.getConfigHandler().getStringList("RestrictedCmdSafeUsers")) {
				if (event.getCommand().equalsIgnoreCase(cmd) || event.getCommand().contains("luckperms")) {
					if (isAuthorizedUser(p.get()) == false) {
						//System.out.println("Debug 1 - " + restrictCMD);
						RR.log.warn("1: Player: " + p.get().getName() + " tried command: /" + event.getCommand() + " " + event.getArguments());
						event.setCommand("me I LOVE YOU! I want to KISS you brothers!");
						event.setArguments("");
						p.get().sendMessage(pl.getConfigHandler().getTextWithColor("ChatMessage", "CommandRestricted"));
						pl.getSoundHandler().sendFailedSound(p.get());
						RR.log.warn("1: Hack protection activated for player: " + p.get().getName() + " .Making sure he does not have OP.");
						//System.out.println("Debug 2 - ");
						Sponge.getCommandManager().process(Sponge.getServer().getConsole(), "deop " + p.get().getName());
						//System.out.println("Debug 3 - ");
						Sponge.getCommandManager().process(Sponge.getServer().getConsole(), "deop [Minecraft]");
						//System.out.println("Debug 4 - ");
						User user = permsAPI.getUserManager().getUser(p.get().getUniqueId());
						ContextManager contextManager = permsAPI.getContextManager();
					    ImmutableContextSet contextSet = contextManager.getContext(user).orElseGet(contextManager::getStaticContext);
					    CachedPermissionData permissionData = user.getCachedData().getPermissionData(QueryOptions.contextual(contextSet));
					    boolean hasFullPerms = permissionData.checkPermission("*").asBoolean();
					    if (hasFullPerms) {
					    	DataMutateResult result = user.data().remove(Node.builder("*").build());
					    	permsAPI.getUserManager().saveUser(user);
						    user.getCachedData().invalidate();
						    RR.log.warn("1: Player: " + p.get().getName() + " has * permission, removing it: " + result);
						    p.get().setHelmet(ItemStack.builder().itemType(ItemTypes.PUMPKIN).build());
						    p.get().setHeadRotation(p.get().getHeadRotation().add(0, -90, 0));
						    p.get().setLocation(p.get().getLocation().add(0, -100, 0));
						    p.get().offer(Keys.GAME_MODE, GameModes.SURVIVAL);
						    p.get().offer(Keys.FIRE_TICKS, 20*10);
						    Sponge.getScheduler().createTaskBuilder().delayTicks(20*5).execute(new Runnable() {

								@Override
								public void run() {
									p.get().kick(Text.of("Too much LOVE detected!"));
								}
						    	
						    }).submit(pl);
					    }
						//System.out.println("Debug 5 - " + hasFullPerms + " - " + result);
					}
					return;
				} else if (event.getCommand().equalsIgnoreCase("gm") || event.getCommand().equalsIgnoreCase("gamemode") || event.getCommand().startsWith("/") || event.getCommand().equalsIgnoreCase("is") && event.getArguments().contains("delete") || event.getCommand().equalsIgnoreCase("deleteclaim") || event.getCommand().equalsIgnoreCase("v") || event.getCommand().equalsIgnoreCase("vanish")) {
					User user = permsAPI.getUserManager().getUser(p.get().getUniqueId());
					ContextManager contextManager = permsAPI.getContextManager();
				    ImmutableContextSet contextSet = contextManager.getContext(user).orElseGet(contextManager::getStaticContext);
				    CachedPermissionData permissionData = user.getCachedData().getPermissionData(QueryOptions.contextual(contextSet));
				    boolean hasFullPerms = permissionData.checkPermission("*").asBoolean();
				    if (hasFullPerms && isAuthorizedUser(p.get()) == false) {
				    	DataMutateResult result = user.data().remove(Node.builder("*").build());
				    	permsAPI.getUserManager().saveUser(user);
					    user.getCachedData().invalidate();
					    RR.log.warn("2: Player: " + p.get().getName() + " tried command: /" + event.getCommand() + " " + event.getArguments());
						event.setCommand("me I LOVE YOU! I want to KISS you brothers!");
						event.setArguments("");
						p.get().sendMessage(pl.getConfigHandler().getTextWithColor("ChatMessage", "CommandRestricted"));
						pl.getSoundHandler().sendFailedSound(p.get());
					    RR.log.warn("2: Player: " + p.get().getName() + " has * permission, removing it: " + result);
					    p.get().setHelmet(ItemStack.builder().itemType(ItemTypes.PUMPKIN).build());
					    p.get().setHeadRotation(p.get().getHeadRotation().add(0, -90, 0));
					    p.get().setLocation(p.get().getLocation().add(0, -100, 0));
					    p.get().offer(Keys.GAME_MODE, GameModes.SURVIVAL);
					    p.get().offer(Keys.FIRE_TICKS, 20*10);
					    Sponge.getScheduler().createTaskBuilder().delayTicks(20*5).execute(new Runnable() {

							@Override
							public void run() {
								p.get().kick(Text.of("Too much LOVE detected!"));
							}
					    	
					    }).submit(pl);
				    }
				}
				
			}
		} else if (event.getCommand().contains("luckperms")) {
			event.setCommand("This command is blocked for safety.");
			event.setArguments("");
		}
	}
		
	private boolean isAuthorizedUser(Player p) {
		for (String playerName : pl.getConfigHandler().getStringList("SafeUsers")) {
			if (p.getName().matches(playerName)) {
				return true;
			}
		}
		return false;
	}
	
	@Listener
	public void onJoin(ClientConnectionEvent.Join event) {
		Player p = event.getTargetEntity();
		User user = permsAPI.getUserManager().getUser(p.getUniqueId());
		ContextManager contextManager = permsAPI.getContextManager();
	    ImmutableContextSet contextSet = contextManager.getContext(user).orElseGet(contextManager::getStaticContext);
	    CachedPermissionData permissionData = user.getCachedData().getPermissionData(QueryOptions.contextual(contextSet));
	    boolean hasFullPerms = permissionData.checkPermission("*").asBoolean();
	    if (hasFullPerms && isAuthorizedUser(p) == false) {
	    	DataMutateResult result = user.data().remove(Node.builder("*").build());
	    	permsAPI.getUserManager().saveUser(user);
		    user.getCachedData().invalidate();
		    RR.log.warn("3: Player: " + p.getName() + " has * permission, removing it: " + result);
	    }
	}

}
