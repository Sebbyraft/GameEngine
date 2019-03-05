package engineTester;

import java.util.ArrayList;
import java.util.List;
import java.util.Random;

import models.RawModel;
import models.TexturedModel;

import org.lwjgl.opengl.Display;
import org.lwjgl.util.vector.Vector3f;

import renderEngine.DisplayManager;
import renderEngine.Loader;
import renderEngine.MasterRenderer;
import renderEngine.OBJLoader;
import terrains.Terrain;
import textures.ModelTexture;
import textures.TerrainTexture;
import textures.TerrainTexturePack;
import entities.Camera;
import entities.Entity;
import entities.Light;
import entities.Player;

public class MainGameLoop {

	public static void main(String[] args) {

		DisplayManager.createDisplay();
		Loader loader = new Loader();
		
		//*************************** TERRAIN TEXTURE PACK ******************************
		TerrainTexture backgroundTexture = new TerrainTexture(loader.loadTexture("grassy"));
		TerrainTexture rTexture = new TerrainTexture(loader.loadTexture("dirt"));
		TerrainTexture gTexture = new TerrainTexture(loader.loadTexture("pinkFlowers"));
		TerrainTexture bTexture = new TerrainTexture(loader.loadTexture("path"));
		TerrainTexturePack texturePack = new TerrainTexturePack(backgroundTexture, rTexture, gTexture, bTexture);
		TerrainTexture blendMap = new TerrainTexture(loader.loadTexture("blendMap"));
		//********************************************************************************
		Terrain terrain = new Terrain(0, -1, loader, texturePack, blendMap, "heightmap");
		//Terrain terrain2 = new Terrain(-1, -1, loader, texturePack, blendMap, "heightmap");
		//********************************************************************************
		
		//*************************** ENTITY STUFF ***************************************
		ModelTexture fernTextureAtlas = new ModelTexture(loader.loadTexture("fern"));
		RawModel fernRawModel = OBJLoader.loadObjModel("fern", loader);
		fernTextureAtlas.setNumberOfRows(2);
		
		RawModel treeModel = OBJLoader.loadObjModel("lowPolyTree", loader);
		
		TexturedModel fern = new TexturedModel(fernRawModel,fernTextureAtlas);
		fern.getTexture().setHasTransparency(true);
		fern.getTexture().setUseFakeLighting(true);
		
		TexturedModel tree = new TexturedModel(treeModel,new ModelTexture(loader.loadTexture("lowPolyTree")));
		tree.getTexture().setHasTransparency(false);
		tree.getTexture().setUseFakeLighting(false);
		
		List<Entity> entities = new ArrayList<Entity>();
		Random random = new Random();
		for(int i=0;i<600;i++){
			if(i % 2 == 0) {
				float x = random.nextFloat()*800; //- 400;
				float z = random.nextFloat()*-800; //* -600;
				float y = terrain.getHeightOfTerrain(x, z);
				entities.add(new Entity(fern, random.nextInt(4),new Vector3f(x,y,z),0,random.nextFloat()*360,0,0.9f));
			}
			
			if(i % 5 == 0) {
				float x = random.nextFloat()*800;
				float z = random.nextFloat() * -800;
				float y = terrain.getHeightOfTerrain(x, z);
				entities.add(new Entity(tree, new Vector3f(x,y,z),0,0,0,1));
			}
			
		}
		//********************************************************************************
		
		//**************************** LIGHT STUFF ***************************************
		Light light = new Light(new Vector3f(0000,10000,-10000),new Vector3f(1,1,1));
		//********************************************************************************
		
		//*************************** PLAYER STUFF ***************************************
		RawModel bunnyModel = OBJLoader.loadObjModel("person", loader);
		TexturedModel stanfordBunny = new TexturedModel(bunnyModel, new ModelTexture(loader.loadTexture("playerTexture")));
		//********************************************************************************
		Player player = new Player(stanfordBunny, new Vector3f(50,0,-100),0, 180, 0, 1);
		//********************************************************************************
		
		
		//*************************** CAMERA STUFF ***************************************
		Camera camera = new Camera(player);	
		//********************************************************************************
		
		MasterRenderer renderer = new MasterRenderer();
		
		while(!Display.isCloseRequested()){
			camera.move();
			player.move(terrain);
			
			renderer.processEntity(player);
			renderer.processTerrain(terrain);
			//renderer.processTerrain(terrain2);
			for(Entity entity:entities){
				renderer.processEntity(entity);
			}
			renderer.render(light, camera);
			DisplayManager.updateDisplay();
		}

		renderer.cleanUp();
		loader.cleanUp();
		DisplayManager.closeDisplay();

	}

}
