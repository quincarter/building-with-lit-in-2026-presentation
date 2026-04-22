update @slides.md , i need to update this with an outline for how to build applications with web components in Lit       
   Element in 2026. Presented by Quin Carter. Starting out, i want to frame the conversation to the user about the          
   frameworks that are out there - React, Angular, Vue, Svelte, and what makes them good for building apps. Ask the         
   audience the question for what they use today. Then dive into the core concepts that make a library useful for building  
   apps - application structure, routing, data flow, state management, and lightweight + versatile. I want to explore the   
   application structure of my existing project tree in my app-shell, options for Routing with Lit-labs router and Vaadin   
   Router, How data flows in the application with lit/context and lit/tasks, using preact-signals for a lightweight state   
   machine, and how the overall project is packaged making it lightweight and versatile.                                    
                                                                                                                            
   Current tree in app-shell:                                                                                               
   ├── README.md                                                                                                            
   ├── biome.json                                                                                                           
   ├── index.html                                                                                                           
   ├── package.json                                                                                                         
   ├── public                                                                                                               
   │   ├── detect-color-scheme.js                                                                                           
   │   ├── pwa-192x192.png                                                                                                  
   │   ├── pwa-512x512.png                                                                                                  
   │   └── vite.svg                                                                                                         
   ├── src                                                                                                                  
   │   ├── app-shell.styles.ts                                                                                              
   │   ├── app-shell.ts                                                                                                     
   │   ├── assets                                                                                                           
   │   │   ├── lit.svg                                                                                                      
   │   │   ├── pwa-192x192.png                                                                                              
   │   │   └── pwa-512x512.png                                                                                              
   │   ├── components                                                                                                       
   │   │   ├── card                                                                                                         
   │   │   │   ├── GenericCard.ts                                                                                           
   │   │   │   ├── README.md                                                                                                
   │   │   │   ├── generic-card.styles.ts                                                                                   
   │   │   │   ├── generic-card.ts                                                                                          
   │   │   │   └── vite-logo.svg.ts                                                                                         
   │   │   ├── chart-js                                                                                                     
   │   │   │   ├── ChartJs.ts                                                                                               
   │   │   │   ├── README.md                                                                                                
   │   │   │   ├── chart-js.ts                                                                                              
   │   │   │   ├── chart-js.utility.ts                                                                                      
   │   │   │   └── chart-types.interface.ts                                                                                 
   │   │   ├── header                                                                                                       
   │   │   │   ├── AppShellHeader.ts                                                                                        
   │   │   │   ├── README.md                                                                                                
   │   │   │   ├── app-shell-header.styles.ts                                                                               
   │   │   │   └── app-shell-header.ts                                                                                      
   │   │   ├── theme-switcher                                                                                               
   │   │   │   ├── README.md                                                                                                
   │   │   │   ├── ThemeSwitcher.ts                                                                                         
   │   │   │   ├── theme-switcher.styles.ts                                                                                 
   │   │   │   └── theme-switcher.ts                                                                                        
   │   │   └── todos                                                                                                        
   │   │       ├── todo-item                                                                                                
   │   │       └── todo-list                                                                                                
   │   ├── index.css                                                                                                        
   │   ├── shared                                                                                                           
   │   │   ├── configuration                                                                                                
   │   │   │   ├── base-path.ts                                                                                             
   │   │   │   ├── mfes.ts                                                                                                  
   │   │   │   ├── nav.ts                                                                                                   
   │   │   │   └── routes.ts                                                                                                
   │   │   ├── contexts                                                                                                     
   │   │   │   ├── accesses.context.ts                                                                                      
   │   │   │   ├── mfe-loader.context.ts                                                                                    
   │   │   │   └── navigation.context.ts                                                                                    
   │   │   ├── interfaces                                                                                                   
   │   │   │   ├── navigation.interface.ts                                                                                  
   │   │   │   └── todos.interface.ts                                                                                       
   │   │   ├── internal-views                                                                                               
   │   │   │   ├── 404-not-found                                                                                            
   │   │   │   ├── no-access                                                                                                
   │   │   │   └── under-construction                                                                                       
   │   │   ├── stores                                                                                                       
   │   │   │   ├── persistent-signal.ts                                                                                     
   │   │   │   ├── todo-list.store.ts                                                                                       
   │   │   │   └── todo.store.ts                                                                                            
   │   │   └── utilities                                                                                                    
   │   │       ├── app-root.utility.ts                                                                                      
   │   │       └── mfe-loader.utility.ts                                                                                    
   │   ├── views                                                                                                            
   │   │   ├── card-examples                                                                                                
   │   │   │   ├── card-examples.ts                                                                                         
   │   │   │   └── card-page.styles.ts                                                                                      
   │   │   ├── chart-examples                                                                                               
   │   │   │   └── chart-examples.ts                                                                                        
   │   │   ├── detail-page                                                                                                  
   │   │   │   └── detail-page.ts                                                                                           
   │   │   ├── home-page                                                                                                    
   │   │   │   └── home-page.ts                                                                                             
   │   │   ├── todos-page                                                                                                   
   │   │   │   ├── todos-page.styles.ts                                                                                     
   │   │   │   └── todos-page.ts                                                                                            
   │   │   ├── view.mixin.ts                                                                                                
   │   │   └── vite-mfe                                                                                                     
   │   │       └── vite-mfe.ts                                                                                              
   │   └── vite-env.d.ts                                                                                                    
   ├── tsconfig.json                                                                                                        
   ├── vite.config.ts                                                                                                       
   └── yarn.lock