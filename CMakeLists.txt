cmake_minimum_required(VERSION 3.0.2)
project(emcl2)

# C++17のフラグを設定
set(CMAKE_CXX_FLAGS "-std=c++17 -O3 ${CMAKE_CXX_FLAGS}")

# 必要なパッケージを探す
find_package(catkin REQUIRED COMPONENTS
  roscpp
  geometry_msgs
  tf2
  tf2_ros
  dynamic_reconfigure
)

# 動的パラメータ設定を生成 (catkin_packageより前に配置)
generate_dynamic_reconfigure_options(
  cfg/emcl2.cfg
)

# catkinパッケージの設定
catkin_package(
  INCLUDE_DIRS include
  LIBRARIES emcl2
  CATKIN_DEPENDS roscpp geometry_msgs tf2 tf2_ros dynamic_reconfigure
)

# インクルードディレクトリの設定
include_directories(
  include
  ${catkin_INCLUDE_DIRS}
)

# ソースファイルに基づいてライブラリを追加
add_library(Mcl_emcl2 src/Mcl.cpp)
add_library(ExpResetMcl2_emcl2 src/ExpResetMcl2.cpp)
add_library(Particle_emcl2 src/Particle.cpp)
add_library(OdomModel_emcl2 src/OdomModel.cpp)
add_library(Pose_emcl2 src/Pose.cpp)
add_library(Scan_emcl2 src/Scan.cpp)
add_library(LikelihoodFieldMap_emcl2 src/LikelihoodFieldMap.cpp)

# 実行可能ファイルを追加
add_executable(emcl2_node src/emcl2_node.cpp)

# 依存関係の追加 (ターゲットが正しく作成されてから呼び出す)
add_dependencies(emcl2_node ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

# 実行可能ファイルに必要なライブラリをリンク
target_link_libraries(emcl2_node
  Mcl_emcl2
  ExpResetMcl2_emcl2
  Particle_emcl2
  Pose_emcl2
  Scan_emcl2
  OdomModel_emcl2
  LikelihoodFieldMap_emcl2
  ${catkin_LIBRARIES}
)
